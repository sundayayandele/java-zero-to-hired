# Capstone defence (viva)

← [Home](../../README.md) · [Phase 1 capstones](../../phases/01-foundations/capstones/README.md) · [Phase 2 capstones](../../phases/02-professional-backend/capstones/README.md) · [Phase 3 capstone](../../phases/03-enterprise-cloud-native/16-capstone-system-design-interviews/README.md)

A capstone counts towards your certificate only after a short **defence**: a 20-minute recorded conversation in which you show that you built it, understand it and can change it. It's the same thing a hiring manager does in a project deep-dive, so it's also your best interview rehearsal. In an age of AI coding assistants it is also how we, and employers, trust that the work is yours. Using AI tools while building is allowed (Module 9 teaches how); not being able to explain or change your own code is not.

## When and how

- After at least one of your capstones has passed its rubric, book a defence with a mentor (or record it yourself, if your cohort uses recorded defences).
- One defence per phase covers **Capstone 1 plus one other capstone of the assessor's choosing**.
- 20 minutes, camera on, screen shared, IDE and terminal open, the deployed service reachable. Recorded for moderation.
- Closed AI: no assistants during the defence. Documentation and your own repository are allowed.

## Format

| Part | Time | What happens |
| --- | --- | --- |
| 1. Demo | 3 min | Show the deployed service doing its main job, and one failure being handled (Phase 2: with a real token) |
| 2. Architecture walk-through | 4 min | Solution → logical → technical view; the main invariant you protect |
| 3. Code questions | 6 min | The assessor picks two files or methods at random and asks you to explain them line by line, and why you chose that design |
| 4. Live change | 5 min | A small change request (for example "change the fee tier boundary", "add a field to the response", "reject transfers on Sundays"): you change the code and a test, and run the tests |
| 5. Trade-offs | 2 min | "What breaks first at 10× load?" or "What would you do differently?" |

## Rubric (15 points, pass 10, no criterion at "Not yet")

| Criterion | Not yet (1) | Competent (2) | Strong (3) |
| --- | --- | --- | --- |
| Authorship | Can't explain code the assessor picks | Explains the chosen code correctly | Explains alternatives and why they were rejected |
| Design reasoning | Describes *what*, not *why* | Justifies the main decisions with the ADRs | Connects decisions to failure modes, data and operations |
| Live change | Can't complete it | Completes it with a test, possibly with hints | Completes it cleanly, test first, explains the impact |
| Testing and failure | No idea how failures are handled or tested | Shows the tests for the main rules and one failure path | Demonstrates concurrency, security or resilience tests convincingly |
| Communication | Disorganised, overruns badly | Clear and within time | Concise, confident, uses diagrams well: interview-ready |

If the defence doesn't pass, you get written feedback and can rebook after 7 days.

## How to prepare

1. Re-read your README, ADRs and threat model (Phase 2) the day before.
2. Pick three random files from your repository and explain them aloud to a friend or a recording.
3. Practise one change request against a timer: change a rule, update the test, run `./mvnw verify`.
4. Prepare your answer to "what breaks first at 10× load?" (topic 5.6).

## Phase 3: the ITL Pay defence

Phase 3 has one capstone, ITL Pay (or the alternative your mentor agreed), so its defence is longer and goes deeper. It follows topic 16.3 and must pass before the Phase 3 certificate.

- **30 minutes**, camera on, screen shared, the deployed `v1.0` reachable, the dashboard and your repository open. Recorded for moderation. Closed AI, as above.
- Book it in week 37, once `v1.0` is tagged and deployed.

| Part | Time | What happens |
| --- | --- | --- |
| 1. Demo | 5 min | A transfer from request to `POSTED` on the deployed system, then one failure handled (for example fraud rejecting it and the hold released), followed in the dashboard and a trace |
| 2. Architecture walk-through | 6 min | The C4 context and container diagrams, the transfer saga, and the three ADRs you'd most want a new teammate to read |
| 3. Code and ADR questions | 7 min | The assessor picks two files and one ADR; you explain them and the alternatives you rejected |
| 4. Live change | 8 min | A small change through the real path: code and test, a pull request, the pipeline, and the deployed system showing it (or a rollback if it breaks) |
| 5. Load and operations | 4 min | Your load-test report's main finding, what breaks first at ten times the peak, and one runbook entry walked through |

It's scored with the same rubric (15 points, pass 10, no criterion at "Not yet"). For **Live change**, "Strong" also means the change reached the cluster through the pipeline, and for **Testing and failure** it means showing a failure in the dashboard or a trace, not only in a test.
