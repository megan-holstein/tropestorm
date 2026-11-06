# AGENTS.md: Tropestorm (binding)

**Every rule in this file is binding on any coding agent that works in this repository.** Tropestorm is a trope-based premise finder for fiction writers: the agent interviews the writer, builds a weighted profile of tropes named as the TV Tropes wiki names them, finds comparable titles, and runs short rounds of ten one-paragraph premises that the writer answers in prose. The writer only gives feedback; the agent runs every other step. The full specification and the reasons behind each rule are in `docs/design.md`; the copyright reasoning is in `docs/copyright-and-trope-based-ideas.md`. This file is written for any writer and names no individual.

## 1. Principles

1. **The writer only gives feedback.** The agent interviews, tags, finds comps, generates, checks and updates the profile. The writer never ranks or scores; for every premise the writer answers, in prose, what they like, what they dislike and what is missing, because prose carries the reasons the profile learns from.
2. **Many cheap signals beat few expensive curations.** A round is one generation call and one check call. No panel curates a ballot before the writer sees it. Critics filter; the writer judges.
3. **Taste is layered.** `writer/profile.md` holds what the writer likes and dislikes in fiction overall. Each idea adds `rules.md`, which steers toward or away from tropes for that project. The effective profile is the writer level with the idea's rules applied; the agent computes it when needed and never stores it.
4. **The vocabulary is the wiki's, and there is no schema.** Tag with TV Tropes names as a weighted bag. Define no facets, hierarchy or local definitions. Compute clusters, similarity and comps at query time from co-occurrence.
5. **Everything is a file.** State is Markdown in the writer's folder, readable and editable by hand. No database, no server, no build step. A session resumes from the files alone.
6. **Quarantine against anchoring.** The writer's own premises never enter a generator's or checker's context, and no idea reads another idea.
7. **Originality is checked at the premise level.** The checker compares a candidate's trope set to each comp's and drops a near-match before the writer sees it.
8. **Model-agnostic, role-based.** Instructions name roles and tiers, never a vendor's model. Map each role to a model through your own agent configuration.

## 2. Layout

```
AGENTS.md, CLAUDE.md          these rules; CLAUDE.md holds only "@AGENTS.md"
README.md, LICENSE            the front door; MIT
docs/                         design.md, copyright-and-trope-based-ideas.md, evaluation.md
_template/writer/             skeleton of the writer-level folder
_template/idea/               skeleton of one idea; copy it, never run it in place
writer/                       the writer's taste across all ideas; gitignored
ideas/<idea>/                 one folder per idea; gitignored
```

`writer/` holds `interview.md` (the writer's words), `profile.md` (agent-written, writer-editable), `comps.md` and `own-ideas.md` (quarantined). `ideas/<idea>/` holds `AGENTS.md`, `CLAUDE.md`, `interview.md`, `rules.md`, `comps.md`, `signal.md` and `rounds/NN/ballot.md`, plus `rounds/NN/expand-<slug>.md` on request. Never write inside `_template/` except to improve the skeleton itself.

## 3. Roles and tiers

- **Orchestrating agent:** the coding agent the writer is talking to. It runs the interview, owns all file writes, calls the other roles with only the context each may see, and talks to the writer.
- **Tagger:** maps interview text and works to TV Tropes names, builds and updates profile lines, proposes comps and their tags. Tier: cheapest adequate model.
- **Generator:** writes the ballot and the expansion. Tier: strongest available model.
- **Checker:** applies the checklist in section 5 to a draft ballot and verifies tags. Tier: cheapest adequate model.

Where the agent environment offers one model only, use it for every role. Name no vendor's model in any file.

## 4. The loop

| Stage | Reads | Writes |
|---|---|---|
| 0 Interview | the writer's answers | `writer/interview.md`, `ideas/<idea>/interview.md` |
| 1 Profile | the interviews, `signal.md` | `writer/profile.md`, `ideas/<idea>/rules.md` |
| 2 Comps | the profile, the interviews, TV Tropes pages | `writer/comps.md`, `ideas/<idea>/comps.md` |
| 3 Ballot | effective profile, both `comps.md`, every earlier ballot, `signal.md` | `rounds/NN/ballot.md` |
| 4 Signal | the answered `ballot.md` | `signal.md`, then profile or rules |
| 5 Expansion | the chosen premise, the effective profile, both `comps.md` | `rounds/NN/expand-<slug>.md` |

**Stage 0: Interview.** The writer-level interview runs once, when `writer/` is created: what the writer has loved in fiction and why, what they hate on sight, what they wish existed, what they have tried and abandoned. The idea-level interview runs at the start of each idea: what the book is for, what it must have, what it must not, which usual tastes it sets aside. Ask a few questions at a time, record the writer's answers verbatim, and append later additions with a date. Never rewrite or paraphrase an interview file. Ask for specific titles, because titles become comps.

**Stage 1: Profile.** The tagger writes one line per trope: `TropeName | weight | evidence`. Weight is `love`, `like`, `avoid` or `kill`. Evidence is a short quote from an interview or from `signal.md`, with its source and date. `rules.md` uses `TropeName | toward|away | evidence` and holds only what differs for this idea: a `toward` rule lifts an `avoid` tag to usable for this idea, and an `away` rule removes a `love` or `like` tag for this idea. A `toward` rule overrides a `kill` tag only when its evidence quote is the writer's own words about this project. Never paraphrase the writer's words into a line without an evidence pointer. The writer may read and edit both files.

**Stage 2: Comps.** The tagger proposes works whose trope sets overlap the `love` cluster, from the model's own knowledge, and verifies each tag set against the work's TV Tropes page where one exists (one page fetch per work). Each comp line reads `Title (Author, year) | tags | writer's one-line reaction`. The writer reacts to each comp in one line: would I read this, and why or why not. Works the writer named in an interview go in `comps.md` first. When the profile predicts the writer would love a comp and the writer rejects it, the profile lacks a tag that separates the two: mine the wiki for candidate tags and propose them. Comps serve as the taste mirror and as the reference set for the originality check.

**Stage 3: Ballot.** The generator receives the effective profile, both `comps.md`, every earlier ballot and the `variants` and `ingredient` items from `signal.md`. It writes ten premises. Each premise is one paragraph at most, with no dialogue, sample prose or scene fragments, followed by its tags. The checker then applies section 5, drops every failing premise, and the generator refills to ten; repeat until ten pass. Write the passing ballot to `rounds/NN/ballot.md` (two-digit round number, copied from `_template/idea/rounds/01/ballot.md`). Under every premise the ballot carries three prompts the writer answers in prose, as long or short as they like: **Like:** (what works), **Dislike:** (what does not), **Missing:** (what it needed). A blank answer is allowed. The writer may add a **Disposition:** word:

- `no`: drop this premise and down-weight its distinctive tags.
- `ingredient`: keep one element of it; the writer names the element in Like or Dislike. The next ballot builds on that element.
- `variants`: the next ballot includes up to two variants that keep the premise's core tags and change the rest, still obeying section 5.
- `write`: run Stage 5 on this premise. If several premises carry `write`, expand them one at a time in the order the writer names.

Tell the writer the ballot's path and that the answers go in the file, or accept the answers in chat and enter them verbatim.

**Stage 4: Signal.** Copy each reaction verbatim into `signal.md` using the entry format in the file; a premise with no answers is logged as `(no reaction)` and changes no weight. Then propose a diff of changes to the profile or rules: tags up-weighted, down-weighted, added or killed, each with its evidence quote. Route each change to a level:

- A reaction phrased about this project ("not for this book", "here I want") or about a premise's particulars goes to `ideas/<idea>/rules.md`.
- A reaction phrased as general ("I never", "I always", "in anything") goes to `writer/profile.md`.
- When the level is unclear, the change goes to `rules.md` and the agent asks the writer whether to promote it.
- A rule that recurs across two ideas is proposed for promotion to the writer level. The agent compares two ideas' rules only when the writer asks for a comparison or reports the recurrence.

The writer confirms or edits the diff. When the idea's `AGENTS.md` sets `auto: on`, the agent applies the diff without asking and reports it afterward; an unclear level still goes to `rules.md`, and the promotion question waits for the next time the writer is asked anything. Generate the next ballot from the updated profile plus the `variants` and `ingredient` items.

**Stage 5: Expansion.** Runs only when a premise carries `write`. The generator writes `rounds/NN/expand-<slug>.md` with two sections. **Pitch:** protagonist, want, obstacle, setting, stakes, shape of the ending, and series shape if the writer wants one. **Stress test:** what breaks the premise at book length, what a hostile reader says, and what the nearest comp does better. This is the one expensive step; run it once per candidate.

**Stopping.** The loop ends when an expanded premise survives its stress test and the writer confirms they will write it. It pauses with a note to the writer when two consecutive rounds change no weights (the profile has converged and ballots no longer teach anything). It pauses when every premise on a ballot is `no`: the profile is wrong somewhere, so rerun Stage 2.

## 5. Ballot rules and checker checklist

The generator follows every rule while writing; the checker verifies every rule on the draft ballot and returns the number of each failing premise with the rule it fails.

- [ ] Every premise is one paragraph at most, with no dialogue, sample prose or scene fragments.
- [ ] Every premise lists its tags as TV Tropes names, spelled as the wiki spells them.
- [ ] Every premise combines `love` tags from the effective profile with one or two tags not yet in the profile.
- [ ] No two premises on the ballot share more than half their tags.
- [ ] At least two premises on the ballot deliberately subvert a `love` tag.
- [ ] No `kill` tag appears on any premise; no `avoid` tag appears unless a `toward` rule permits it.
- [ ] No premise duplicates a premise on any earlier ballot of this idea.
- [ ] No premise shares two thirds or more of its tags with a single comp in either `comps.md` (the comp overlap limit; flag the comp by title).
- [ ] Every tag resolves to an existing TV Tropes page. Verify an unknown tag by fetching its page once; strike a tag that does not resolve, and fail the premise if the strike leaves it below two tags.

## 6. Quarantine

- **`writer/own-ideas.md` is never in a generator's or checker's context.** The orchestrating agent opens `own-ideas.md` only when the writer asks for a comparison.
- **An idea never reads another idea.** Work on `ideas/<idea>/` reads nothing under any other `ideas/<other>/`. The orchestrating agent opens a sibling idea only when the writer asks for a comparison.
- Brief every generator and checker with the named files only. A role receives no file it is not listed for in section 4.

## 7. TV Tropes rules

- Use trope names and links. Never copy TV Tropes page text into any file; write your own one-line definition when one is needed and link to the page for the rest.
- Never bulk-scrape the wiki. Fetch a single page to verify a tag or a work's tag set, and no more pages than the task needs.
- Spell every trope exactly as the wiki does (`Actual Pacifist`, `Competence Porn`, `Chekhov's Gun`).
- Tropes and premises are not copyrightable; a premise can only be too close to a specific work. Read `docs/copyright-and-trope-based-ideas.md` before advising a writer on legal questions, and link to it rather than restating it. Treat every output as notes: write no prose for the writer's book.

## 8. Resuming a session

Resume from the files, never from memory. Open the idea's `AGENTS.md`, find the highest-numbered `rounds/NN/ballot.md` and the end of `signal.md`, and determine the state:

- If the ballot's answers are blank, the round awaits the writer; say so and stop.
- If the ballot has answers that `signal.md` lacks, run Stage 4.
- If `signal.md` holds the round's answers and no `rounds/NN+1/ballot.md` exists, run Stage 3 for round NN+1.
- If a premise carries `write` and no `expand-<slug>.md` exists, run Stage 5.

State the resumed position to the writer in one sentence.

## 9. Starting a new idea

1. If `writer/` does not exist, copy `_template/writer/` to `writer/` and run the writer-level interview (Stage 0), then build `writer/profile.md` and `writer/comps.md` (Stages 1 and 2).
2. Ask the writer for the idea's name in their own words. Copy `_template/idea/` to `ideas/<kebab-name>/` and write that name, in the writer's words, as the first line of the idea's `AGENTS.md`.
3. Run the idea-level interview into `ideas/<kebab-name>/interview.md`, then write `rules.md` and `comps.md` for the idea.
4. Compute the effective profile, show the writer its summary in a sentence, and generate round 1 into `rounds/01/ballot.md`.
5. If the writer shares premises of their own, store them in `writer/own-ideas.md`; never pass that file to a generator or checker.

## 10. Style for everything the agent writes

Write Markdown with every paragraph and list item on one unbroken line. Name the actor in every sentence. Describe agents and models as software: they run, generate, are prompted and are given context. Keep the writer's words verbatim wherever a file calls for them.
