## What you will be able to do

- Freeze features and turn everything left over from `v0.1`–`v0.6` into a gap list, sorted by risk.
- Remove the starter's shortcuts that a reviewer will spot first: in-memory balances, a global lock, a header anyone can set.
- Add contract tests between services and a smoke test that runs against the deployed system.
- Write release notes, tag `v1.0` and know what "done" means for a system rather than a feature.
- Map ITL Pay's patterns onto loan origination, a marketplace or a logistics backend.

## Why this matters in a bank or fintech

Banks don't hire people who can start systems; they have plenty of those. They hire people who can finish them: close the edge cases, make the tests trustworthy, and hand over something another team can run at 3 a.m. A hiring manager reading your repository will look for the same things a reviewer at a bank looks for before go-live: what happens to money when something fails, what's tested, and what you knew was missing. A short, honest list of known limitations impresses more than a long list of features.

## 1. Feature freeze

At the start of week 36, stop adding features. From now on a change goes in only if it fixes a defect, closes a gap on your list, or makes the system easier to run. Everything else goes into a "next" section of your README.

Freezing feels like giving up. It isn't. Every real release has a freeze, because new code is where new defects come from, and you need the remaining time for the work that turns a demo into a system: tests, deployment, docs and load.

## 2. The gap list

Go through your ADRs, your module briefs, your `TODO` comments and the issues in your repository, and write every known gap in one table:

| Gap | Risk if left | Effort | Decision |
| --- | --- | --- | --- |
| Accounts' balances in memory | All balances lost on restart; can't run two replicas | 1 day | **Must** |
| `synchronized` hold for all accounts (13.5) | Lock contention at peak; breaker opens | Half a day | **Must** |
| Gateway trusts `X-Customer-Id` from the client | Any customer reads any account through the assistant | 2 hours | **Must** |
| Stuck-transfer sweeper has no metric | Stuck money invisible until a customer complains | 1 hour | **Should** |
| Notifications only log the SMS | No real SMS provider | – | **Won't** (known limitation) |
| Rates provider is simulated | Quotes aren't real prices | – | **Won't** (known limitation) |

Sort by risk, not by how interesting the work is. Three rules decide "must":

1. **Money.** Anything that can lose, duplicate or misreport money: idempotency, the ledger balancing, compensations, rounding.
2. **Security.** Anything that lets one customer see or move another's money, or puts a secret in Git.
3. **Running it.** Anything that stops someone else deploying, observing or recovering the system.

The "won't" items go into a **Known limitations** section of your README, each with a sentence on what you'd do with more time. A reviewer who finds a gap you listed thinks "this engineer knows their system". One who finds a gap you didn't thinks the opposite.

## 3. Removing the starter's shortcuts

The starter took shortcuts so each lesson could focus on one idea. By `v1.0` none of these should be left:

- **Balances in PostgreSQL**, with Flyway migrations, the `hold` table's `UNIQUE (account_id, reference)` (topic 10.4), and at least two replicas of accounts running.
- **Per-account locking** instead of the global `synchronized` method: a row lock on the account inside the hold transaction, so two transfers from different accounts never wait for each other:

```java
@Transactional
public HoldResult hold(String accountId, String reference, BigDecimal amount) {
    var existing = holds.findByAccountIdAndReference(accountId, reference);
    if (existing.isPresent()) {
        return HoldResult.replayed(existing.get());          // idempotent: same reference, same answer
    }
    Account account = accounts.findByIdForUpdate(accountId)  // SELECT ... FOR UPDATE: locks this row only
            .orElseThrow(() -> new AccountNotFound(accountId));
    account.reserve(amount);                                 // throws InsufficientFunds
    return HoldResult.created(holds.save(new Hold(accountId, reference, amount)));
}
```

```java
@Lock(LockModeType.PESSIMISTIC_WRITE)
@Query("select a from Account a where a.id = :id")
Optional<Account> findByIdForUpdate(String id);
```

- **The customer from the token.** The gateway validates the JWT, removes any `X-Customer-Id` the client sent and sets it from the token's subject; every service still validates the token itself (defence in depth, Phase 2 topic 6.2).
- **The ledger in the saga**, posting balanced journals for every `CLEARED` transfer, with `POSTED` as the final state (Module 11).

Prove each one with a test, not a sentence in the README. The test for the gateway header is the one reviewers like most: a request with someone else's `X-Customer-Id` and a valid token for yourself must see only your own data.

## 4. Tests that let you change the system

By now each service has unit tests and Testcontainers tests. Two kinds are usually missing at this point, and both matter for a system of services.

**Contract tests.** Transfers calls accounts' hold endpoint. If accounts renames a field, transfers' own tests still pass (they use a stub), and the system breaks in production. A **consumer-driven contract** fixes that: the consumer (transfers) writes down what it sends and expects, and the provider (accounts) runs that contract in its own build. Spring Cloud Contract and Pact are the two common tools. A lighter version that's enough for `v1.0`: keep the request and response examples in one shared folder, use them as WireMock stubs in transfers' tests, and replay them against the real controller in accounts' tests. Either way, a breaking change fails the provider's build, before it's deployed.

**A smoke test against the deployed system.** A few requests that prove the main journey works end to end, run by the pipeline after every deployment and by you before the defence:

```bash
#!/usr/bin/env bash
# smoke.sh BASE_URL TOKEN: one transfer from start to POSTED, through the public route
set -euo pipefail
ref="SMOKE-$(date +%s)"
curl -fsS -X POST "$1/api/transfers" -H "Authorization: Bearer $2" -H 'Content-Type: application/json' \
     -d "{\"reference\":\"$ref\",\"from\":\"SMOKE-A\",\"to\":\"SMOKE-B\",\"amount\":100.00}" > /dev/null
for i in $(seq 1 30); do
  state=$(curl -fsS "$1/api/transfers/$ref" -H "Authorization: Bearer $2" | jq -r .state)
  [ "$state" = "POSTED" ] && { echo "smoke: $ref POSTED after ${i}s"; exit 0; }
  [ "$state" = "FAILED" ] && { echo "smoke: $ref FAILED"; exit 1; }
  sleep 1
done
echo "smoke: $ref still $state after 30s"; exit 1
```

Use dedicated smoke-test accounts, never real customers', and make sure their transfers are easy to find and exclude from reports. Keep the smoke test short: its job is to answer "is the main journey working right now?", not to replace the test suite.

## 5. Done, for a system

A feature is done when it's merged with tests. A **system release** is done when someone else could run it. Use this checklist for `v1.0`:

- [ ] `./mvnw verify` passes from a clean clone; the README's quick start works on a machine that has never seen the project.
- [ ] Every service has a database of its own, migrations, health probes, limits and structured logs with trace ids.
- [ ] No secret in Git (run gitleaks over the whole history, not just the last commit).
- [ ] Dependency and image scans pass with no critical findings, or each one is written down with a reason.
- [ ] The pipeline deploys to a running cluster and the smoke test passes there.
- [ ] Every paging alert has a runbook entry, and you've tried each runbook at least once.
- [ ] The load-test report is in the repository, with the numbers of the deployed version.
- [ ] The README has the diagrams, the ADR index and the known limitations.

## 6. Release notes and the tag

A **changelog** lists what changed, for people who use or run the system. Keep it in `CHANGELOG.md`, newest first, grouped as *Added*, *Changed*, *Fixed* and *Known limitations*. Write for a reader, not from the commit log: "Transfers from different accounts no longer wait for each other (per-account locking)" is useful; "refactor AccountService" isn't.

Tag the release from `main` after the pipeline is green:

```bash
git tag -a v1.0 -m "ITL Pay v1.0: deployed, documented, load-tested"
git push origin v1.0
```

With **semantic versioning**, `1.0` says the public API is now stable: a breaking change to it would need `2.0`. That's why the tag waits until the API, the events and their schemas are the ones you're prepared to defend.

## 7. Another domain: the same patterns

If you built an alternative system, or are asked in an interview how ITL Pay's design would carry over, map the patterns rather than the nouns:

| ITL Pay | Loan origination | Marketplace | Logistics |
| --- | --- | --- | --- |
| Transfer saga with compensation | Application → checks → decision → disbursement; a declined offer releases nothing, a failed disbursement reverses the booking | Order → stock reservation → payment → delivery; cancel releases stock and refunds | Job → match → pickup → delivery; a cancelled job frees the driver |
| Idempotent hold on an account | Idempotent disbursement by application id | Idempotent payment and payout by order id | Idempotent assignment by job id |
| Fraud screening consumer | Credit-bureau and affordability checks | Seller and buyer risk checks | Driver and route checks |
| End-of-day interest batch | Daily arrears and repayment schedule batch | Daily seller payout batch | Daily driver settlement batch |
| FX quote valid for 30 seconds | Offer valid for 7 days | Price held in the basket for 15 minutes | Fare quote valid for 2 minutes |
| Rates stream (WebFlux) | – | Live stock levels | Driver locations at high rate |

The hard parts are the same everywhere: money and state that must not be lost or duplicated, steps that fail halfway, and things that expire.

## Worked example: triaging the last week

It's Monday of week 36 and Amaka's gap list has 14 items. She has about 22 hours. She sorts them:

- **Must (5 items, about 2.5 days):** accounts in PostgreSQL with two replicas; per-account locking; the customer taken from the token at the gateway; the ledger in the saga up to `POSTED`; gitleaks over the history (it finds a Keycloak client secret committed in week 29, so she rotates the secret first, then moves it out of the files; rewriting history alone doesn't make a pushed secret safe).
- **Should (4 items):** contract tests between transfers and accounts; the sweeper's metric; a smoke test; the changelog. She does the smoke test and the contract tests, because the pipeline in 16.2 needs them.
- **Won't (5 items):** a real SMS provider, real FX prices, a second region, multi-currency accounts and a web front end. Each gets a line under *Known limitations*.

On Thursday she stops coding and starts topic 16.2. Two "should" items are left; she writes them down too.

## Common mistakes

- Adding a feature in the last week "because it's small". It never is, and it's what fails in the demo.
- Leaving the starter's shortcuts in place and hoping no one looks.
- A gap list sorted by interest instead of by risk to money, security and operations.
- Hiding known limitations instead of writing them down.
- Only testing services in isolation, so a renamed field breaks the system after deployment.
- A smoke test that uses real customers' accounts, or that takes ten minutes.
- Removing a committed secret from the files without rotating it.
- A changelog copied from commit messages.

## Practice

1. Write your gap list in the format of section 2 and agree the "must" items with your mentor.
2. Replace the global lock with per-account locking and write a test with two threads holding on different accounts at the same time; then run the 13.5 load test again and compare.
3. Add a consumer-driven contract between transfers and accounts (Spring Cloud Contract, Pact or shared examples), and show that renaming a field in accounts fails accounts' build.
4. Write `smoke.sh` (or the same as a JUnit test tagged `smoke`) and run it against your kind cluster.
5. Run gitleaks over your repository's whole history and act on what it finds.
6. Write the *Known limitations* section of your README, one or two sentences per item.

## Key terms

Feature freeze, gap list, known limitation, risk, must/should/won't, per-account locking, `SELECT ... FOR UPDATE`, pessimistic lock, consumer-driven contract, Spring Cloud Contract, Pact, provider and consumer, smoke test, definition of done, changelog, semantic versioning, annotated tag, secret rotation.

## Further reading

- [Martin Fowler: Consumer-driven contracts](https://martinfowler.com/articles/consumerDrivenContracts.html)
- [Spring Cloud Contract reference](https://docs.spring.io/spring-cloud-contract/reference/)
- [Pact documentation](https://docs.pact.io/)
- [Keep a Changelog](https://keepachangelog.com/en/1.1.0/)
- [Semantic Versioning 2.0.0](https://semver.org/)
- [gitleaks](https://github.com/gitleaks/gitleaks)
