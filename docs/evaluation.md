# Evaluation protocols

Tropestorm claims that interview, profile and ballot rounds produce a premise a writer wants to write faster and better than a single good chatbot prompt. This document defines two tests that check the claim before any further build. Results go in the README under "Evidence" as raw numbers with the protocol version, not as marketing.

## Test A: ballot quality against a baseline chatbot prompt

**Question.** With the same writer, does interview plus first ballot produce a premise the writer rates above what a single good chatbot prompt produces?

**Participants.** Six to eight volunteer writers who write or intend to write genre fiction, have not used Tropestorm and have not seen its ballots. Each participant gives informed consent for the use of their anonymized ratings. Each uses their own coding agent and a chatbot with no stored memory of them.

**Materials.**

- A fresh clone of this repository, run by the participant's coding agent.
- A fresh chatbot session with no memory, no custom instructions and no files attached.
- The baseline prompt below, pasted verbatim.
- A rating sheet: a table with one row per premise and a column for a 1 to 7 score (1 is "I would never write this", 7 is "I would start this today"). The loop itself uses prose, never numbers; this sheet exists only to compare the two arms.

**Baseline prompt (arm B).**

```
I am a fiction writer looking for a premise for my next book. Before you suggest anything, interview me: ask me what I love in fiction and why, what I hate on sight, what I wish existed, and what I have tried and abandoned. Ask a few questions at a time. When you have enough, give me ten original book premises, each at most one paragraph long, with no dialogue or sample prose. Make the premises varied, avoid cliches, and make sure none copies a published work too closely.
```

**Procedure.**

1. Randomize each participant's arm order (A first or B first) and record it.
2. In arm A, the participant completes the writer interview, the idea interview, comps and round 1 as `AGENTS.md` directs, with a 45-minute limit. In arm B, the participant answers the chatbot's interview questions and receives ten premises, with the same 45-minute limit.
3. A different person (or a script) strips every tag, heading and formatting cue from both sets, labels each premise with a random code, shuffles all twenty and hands them to the participant.
4. The participant scores all twenty premises on the 1 to 7 sheet without knowing the source of each.
5. After scoring, the participant guesses which set came from which arm, and states in one sentence what they would change about their best premise.

**Measured.**

- Best score per arm for each participant.
- Number of premises scoring 5 or higher per arm for each participant.
- Time spent per arm.
- Whether the participant guessed the arms correctly.

**Reporting.** State the success threshold before running: arm A wins if the best score in arm A exceeds the best score in arm B for a majority of participants and the median paired difference of the best score is at least one point. Report the number of participants, every participant's pair of best scores, the median and range of the paired difference, the count of premises scoring 5 or higher per arm, and the time per arm. Report every participant; exclude none. State plainly that the sample is small and the result is indicative, not statistical.

## Test B: rounds to a `write` premise

**Question.** How many rounds does it take a volunteer to mark a premise `write`?

**Participants.** The same volunteers, continuing from arm A of Test A, plus any additional volunteers who start from the interviews.

**Materials.** The participant's own `ideas/<idea>/` folder after Test A. No further materials.

**Procedure.**

1. The participant answers round 1 as the loop directs and continues round by round, with the agent applying the profile deltas after each round.
2. The participant stops at the first round in which any premise carries `write`, or after round 8, whichever comes first.
3. For each round, record the wall-clock time the participant spent answering, the number of model calls the agent made, and whether the loop paused under its stopping rules (two rounds with no weight change, or a ballot of all `no`).
4. If a premise carries `write`, the participant reads the expansion and states whether they will write it.

**Measured.**

- Rounds until the first `write` (or "not reached by round 8").
- Participant time and agent calls per round.
- Whether the expanded premise survived its stress test and the participant confirmed it.
- Whether the loop paused, and at which stopping rule.

**Reporting.** The target is a handful of rounds, not a dozen: a median of five or fewer rounds to the first `write`. Report the participant count, each participant's round count, the median and range, the number who did not reach `write` by round 8, the median participant time per round, and the median agent calls per round. Report every participant; exclude none.

## Recording results in the README

Add a table under "Evidence" in `README.md` with one row per test: the protocol version (the date of this file), participant count, the headline number from the measures above, and a link to the raw data in `docs/`. Raw data holds anonymized scores and counts only; it never holds profiles, premises that a participant wants kept private or interview text. Until a test has run, the README states that no results exist.
