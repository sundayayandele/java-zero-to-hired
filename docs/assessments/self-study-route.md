# The self-study route to your certificate

← [Home](../../README.md) · [Capstone defence](capstone-defence.md) · [System-design interview](system-design-interview.md)

Every phase has two certificates. You can earn either; the work you do for both is mostly the same.

| | **Certificate of Completion (Self-Study)** | **Certificate of Completion (Mentor-Verified)** |
| --- | --- | --- |
| Cost | Free | Mentorship membership or mentor hours |
| Topic quizzes, module projects, capstones, practice checkpoint, exam | Required | Required |
| Showing the capstone is yours | A **recorded defence** on GitHub, plus **code checks** | A live defence with a mentor |
| Coding under time pressure (Phases 1 and 2) | A **timed coding test** on itlearning.tech | A live-coding assessment with a mentor |
| System design and interviews (Phase 3) | **Recorded answers** on GitHub | A live system-design interview and two live mock interviews |
| Feedback | A mentor quick-checks each recording within **15 working days** | Live feedback from your mentor |

You can start on the self-study route and upgrade later: your quizzes, projects, capstones and exam carry over, and you only book the live assessments.

## 1. Code checks

Copy [`templates/code-checks/.github/workflows/itl-code-checks.yml`](../../templates/code-checks/.github/workflows/itl-code-checks.yml) into `.github/workflows/` in your capstone repository and push. GitHub Actions then checks three things on every push:

- **Build and tests:** `./mvnw verify` passes, and at least 15 tests run.
- **No secrets:** gitleaks finds no passwords, keys or tokens anywhere in the history. If it finds one, rotate the secret first, then remove it.
- **Work spread over time:** commits on at least 10 different days, over at least 3 weeks. Commit as you go; don't squash your history into one commit.

When all three are green, copy the link to that run (*Actions* tab → the run → copy the address).

## 2. The recorded defence

Record yourself (screen and voice; camera on if you can) for **15–20 minutes**, in this order:

1. **Demo (3 min):** your deployed service doing its main job, and one failure being handled.
2. **Architecture (4 min):** your diagrams and the main rule your system protects, for example "money is never created or lost".
3. **Code (5 min):** open two files you're proud of and explain them line by line, and why you designed them that way.
4. **Change request (5 min):** make the change request assigned to you (below), with a test, and run the tests on screen.
5. **Trade-offs (2 min):** what breaks first at 10 times the load, and what you'd do differently.

Rules: no AI assistant on screen during the recording; one continuous take, no cuts in the change-request part; your face or voice must be the same in the whole recording.

### Your change request

Take the **last digit of your user ID on itlearning.tech** (your profile page address ends in `id=…`) and do the matching change for your phase:

| Last digit | Phase 1 | Phase 2 | Phase 3 (ITL Pay) |
| --- | --- | --- | --- |
| 0 or 5 | Reject any transaction above a new configurable limit, with a clear error | Add a new role that can read but not change data, and protect one endpoint with it | Add a daily transfer limit per customer, enforced in the transfers service |
| 1 or 6 | Add a new field to the main entity, stored and returned by the API | Add a filter parameter to a list endpoint, with paging still working | Add a new transfer status `ON_HOLD` that fraud can set and support can release |
| 2 or 7 | Change a calculation rule (a fee, interest or rounding rule) and its tests | Return Problem Details for a new validation error | Publish a new event when a transfer fails, and consume it in notifications |
| 3 or 8 | Add an endpoint that returns a summary (a count or total) | Make one write endpoint idempotent with an `Idempotency-Key` header | Add a metric and a dashboard panel for a new business number |
| 4 or 9 | Reject requests on a configurable list of blocked days or accounts | Add a timeout and retry with backoff to an outgoing call | Add a feature flag that turns a feature off without a redeploy |

### Where the recording lives: GitHub only

Don't upload videos to itlearning.tech. Put the recording in your capstone repository:

- **Recommended:** create a GitHub **release** (for example `defence-1`) and attach the video file to it (releases accept files up to 2 GB).
- **Or** commit it under `docs/defence/` if it's under 100 MB (export at 720p).

Then add a **Defence** section to your README with the link to the video, the change request you did (its letter and digit) and the commit with the change. Keep the repository public, or share it with the mentors, until your certificate is issued.

## 3. Timed coding test (Phases 1 and 2)

On itlearning.tech, in the end-of-phase section: three Java problems in **60 minutes**, auto-graded in the browser like the coding practice quiz. Pass with 2 of the 3. You can retake it after 7 days. No IDE, no AI assistants and no other tabs: treat it as an exam.

## 4. Recorded system design and interview answers (Phase 3)

Record these the same way and link them from your README:

- **System design (25–30 minutes):** design the system matching the last digit of your user ID, following the five steps in [topic 16.4](../../phases/03-enterprise-cloud-native/16-capstone-system-design-interviews/16.4-system-design-interviews/README.md) (clarify, estimate, design, deep dive, wrap up) on a whiteboard tool, thinking aloud. 0–1: a card authorisation service. 2–3: a savings-goal app with round-ups. 4–5: merchant settlement and payouts. 6–7: a loan repayment scheduler with arrears. 8–9: a peer-to-peer payments app with request-to-pay.
- **Interview answers (about 20 minutes):** answer each of these aloud, in about two minutes each, as you would in an interview ([topic 16.5](../../phases/03-enterprise-cloud-native/16-capstone-system-design-interviews/16.5-mock-interviews/README.md)): tell me about ITL Pay; a difficult bug you fixed; a decision you made with incomplete information; a time you were wrong; two transfers debit the same account at once, what can go wrong and how do you prevent it; p99 latency doubled after a release, what do you do; how do you avoid processing a Kafka message twice?

## 5. Submitting and the quick check

On itlearning.tech, submit **Recorded defence and code checks (self-study)** with the links to your README section, the video and the green code-checks run (Phase 3 has two more submissions, for the recorded system design and interview answers). A mentor checks each submission within **15 working days**: is it you, is it your repository, does the change work, and do you understand what you show. If something's missing, you get a note and can submit again; there's no fee either way.

When everything for your phase has passed, your self-study certificate is issued and emailed automatically.
