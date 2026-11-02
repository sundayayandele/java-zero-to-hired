# System-design interview

← [Home](../../README.md) · [Phase 3](../../phases/03-enterprise-cloud-native/README.md) · [Topic 16.4](../../phases/03-enterprise-cloud-native/16-capstone-system-design-interviews/16.4-system-design-interviews/README.md)

In week 37 of Phase 3 you design a system you haven't seen before, with a mentor playing the interviewer. It's scored with the same things employers look for in intermediate backend design rounds. It's practice for real interviews, and it counts towards your Phase 3 certificate.

## When and how

- Book it after topic 16.4, in week 37, with a mentor.
- 45 minutes on video, camera on, with a shared whiteboard (Excalidraw, draw.io or paper held to the camera). Recorded for moderation.
- The mentor picks one problem from a private list of banking and fintech systems (for example a card authorisation service, a savings-goal app, a merchant settlement system or a loan repayment scheduler). You won't know it in advance.
- Closed AI: no assistants. You may use a calculator and your own notes on estimation.

## Format

| Part | Time | What happens |
| --- | --- | --- |
| 1. Clarify | 5 min | You ask about users, features, scale, money correctness, consistency, availability and regulation |
| 2. Estimate | 5 min | You turn the answers into numbers: peak rate, storage, read-to-write ratio |
| 3. High-level design | 10 min | You draw the main parts and walk one request through them |
| 4. Deep dive | 20 min | The mentor steers you into one or two hard parts and adds a constraint or a failure |
| 5. Wrap up | 5 min | Bottlenecks, failure modes, monitoring and what you'd do next |

## Rubric (15 points, pass 10, no criterion at "Not yet")

| Criterion | Not yet (1) | Competent (2) | Strong (3) |
| --- | --- | --- | --- |
| Requirements | Starts drawing without asking | Asks about features and scale, writes them down | Also asks about money correctness, consistency, audit and failure; uses the answers later |
| Estimation | No numbers, or numbers that don't change anything | Reasonable peak rate and storage, stated aloud | Numbers drive decisions (cache, partitioning, queues) and are rechecked when facts change |
| Design | Boxes without a request flowing through them | A working design, one request walked end to end, patterns named | Simple first, deepened where it matters; idempotency, outbox, sagas and consistency used correctly |
| Trade-offs and failure | No alternatives; failure ignored | States the main trade-offs and handles the obvious failures | Compares options with their costs, designs for timeouts, retries, duplicates and reconciliation |
| Communication | Silent, or lost track of time | Thinks aloud, checks in, finishes on time | Structured and calm, adapts to new facts, links points to real experience such as the capstone |

If the interview doesn't pass, you get written feedback and can rebook after 7 days, with a different problem.

## How to prepare

1. Work through topic 16.4 and run the three designs in its section 6 against a 45-minute timer.
2. Practise back-of-the-envelope estimates until you can do them aloud without hesitating.
3. Do one practice interview with a peer, each of you scoring the other with this rubric.
4. Write down three real numbers from your ITL Pay load-test report (topic 16.3) you could use to support a decision.
