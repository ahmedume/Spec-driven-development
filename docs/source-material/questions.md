# Questions to Ask After the Build

*A question bank for the moment an AI agent says "done". Ask these, in order. They are designed to pull out what the agent actually built — the concept, the mechanism, the trade-offs — not what it *thinks* you want to hear.*

**How to run it:** don't accept "done" as an answer. Ask in order, take notes verbatim, and don't accept paraphrased restatements of the feature description as answers. Every vague answer collapses on the next question.

---

## A. The concept — what did it build, exactly?

1.  In one sentence, what does this application do?
2.  What problem does it solve, and for whom?
3.  What did it decide to build that you did *not* ask for?
4.  What did it decide not to build, and why?
5.  What is the single most important user action it supports?
6.  What is the worst thing this application can do to a user right now, and how would you know?
7.  What would make a user come back a second time?

## B. The mechanism — how does it actually work?

8.  Trace one request from the UI to the response. Every step, in order.
9.  Which models does it use, in which roles — and why those models rather than smaller or larger ones?
10. What state does it hold, where, and who can read and write it?
11. What happens when an external dependency (model, API, database) times out or returns an error?
12. What input does it accept, and what validation happens before anything trusts it?
13. Where are the decision points in the code, and what inputs drive them?
14. If this produces a score or a ranking, what is the mechanism behind it — and where is the calibration?
15. If this produces generated text, what does the explanation depend on, and is it derived from the same computation as the answer?

## C. Architecture and trade-offs

16. What alternatives were considered and rejected, and why?
17. What are the trade-offs — what got worse to get this done?
18. What breaks first under 10,000 concurrent users, and what did you design in to avoid it?
19. Which part of this is the weakest — and what would make it the incident?
20. What technical debt did you introduce, and when does it come due?
21. What would change if this were rebuilt in three months for the first paying customer?
22. If this were an interview, how would you justify the two biggest decisions?

## D. Data, security, and privacy

23. What data does it collect, where is it stored, and how long is it kept?
24. Who can access what, and where is the permission check enforced?
25. What happens if a secret leaks — is it in the repo, in the logs, in the config?
26. What input validation is missing?
27. What does it do with user-provided files — upload checks, size limits, content sniffing?
28. What does the dependency audit show right now?

## E. Testing and quality

29. Which parts are tested, and which are only verified by hand?
30. Which edge cases were considered: empty input, huge input, concurrent writes, partial failures?
31. What is the smallest change a user could make that would break production?
32. How would you know within five minutes of an incident?
33. What did you *not* test?

## F. Operating it

34. How is this deployed — what is the release process from this machine?
35. What happens at 3am if it falls over — what alerts, what logs, what runbook?
36. What is the cost per month at current usage — and at 100x usage?
37. What dependency is most likely to break first?
38. What would you tell a new teammate about this codebase in ten minutes?

## G. The review — does it deserve to exist?

39. What would you change if you had to build it again?
40. What would you delete right now, knowing what you know?
41. What is this product's one sentence, to someone who has never seen it?
42. Would a target user, using it for a week, notice it exists? Why or why not?

---

## Worked example — an AI-content detector

The seven original questions from the source material, as they read against a real brief. Every AI-product brief needs these:

1.  **How does it detect AI-generated content?** — the signal, the model, the training data, the failure cost.
2.  **How precise is the score?** — the scale, the calibration, the threshold, the test set and the base rate.
3.  **What technologies detect it?** — versions, vendors, costs, contract boundaries.
4.  **Does the output relate to the source material?** — evidence spans; input contract; faithfulness.
5.  **Why this framework over the simplest alternative?** — the rejected alternatives named.
6.  **Define its logic, workflow, and models used.** — three sentences or it isn't designed.
7.  **Elaborate the tech stack and why.** — layer by layer, with the rejected alternative attached to every choice.

The structured guide built from these questions is [`../09-spec-interrogation.md`](../09-spec-interrogation.md) — read it when the interrogation gets stuck.

---

## How to tell a real answer from a comfortable one

| Real answer | Comfortable answer |
| --- | --- |
| Names a mechanism, a file, a table, a test | Names an intention or a capability |
| Has a number, a span, a threshold | Has an adjective |
| Admits what's untested or unknown | Says nothing could go wrong |
| Points at something you can open and check | Nothing to check |
| Gets *shorter* the harder you push | Gets *longer* the harder you push |

If you're standing at "done", ask these for ten minutes. The agent either knows its own build, or it doesn't — and now you know which.
