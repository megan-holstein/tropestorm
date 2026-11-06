# Tropestorm: system design

Tropestorm is a trope-based premise finder for writers. Status: approved design, 2026-11-05; the writer's decisions of that date are folded in (name, MIT license, repository in this folder, feedback format, one-paragraph pitches, two-level profile). This document is the specification of the system that will be published on GitHub. It describes what the system is, the decisions that shape it and the alternatives rejected, so that a reader can judge the design before anything is built and a contributor can change it later without re-deriving it.

## 1. What it is

A repository of Markdown files and agent instructions that a writer runs inside their own coding agent (Claude Code, Codex, Gemini CLI, or any agent that reads `AGENTS.md`). It interviews the writer about what they love and hate in fiction, translates the interview into a weighted profile of **tropes** named as TV Tropes names them, finds **comparable titles** by trope overlap, and then runs short, cheap rounds: ten premises of at most a paragraph each, the writer answers three questions about each (what they like, what they don't, what's missing), the profile updates, the next ten come closer. The profile has two levels: what the writer likes in fiction overall, and what this particular project is for. When the writer says "I'd write this," one premise is expanded and stress-tested. The writer only reacts; the agent runs everything else.

It is not a writing tool, a plot generator or a chatbot wrapper. It writes no prose for the book, it keeps a persistent profile the writer can read and edit, and it stops when the writer has a premise they want to write.

## 2. Principles (the decisions everything else follows from)

1. **The writer only gives feedback.** Every other step (interviewing, tagging, finding comps, generating, checking, updating the profile) is the agent's. The writer never ranks or scores; for every premise they answer, in prose, what they like, what they don't like and what's missing. Prose carries the reasons, and reasons are what the profile learns from; a ranking would lose them.
2. **Many cheap signals beat few expensive curations.** A round is one generation call and one check call, and a writer can run several a day. Nothing is curated by a panel before the writer sees it. This is the remedy for the cold-start problem, and the research literature agrees that model judges rank originality badly (r≈0.16 against humans) while filtering adequately, so critics filter and the writer judges.
3. **Taste is layered: writer-level, then project-level.** A writer-level profile holds what they like and dislike in fiction overall, and carries across every idea. Each idea adds project rules on top that steer toward or away from tropes for what the writer wants out of that project (a project may want a trope the writer usually avoids, or forbid one they love). The effective profile for a round is the writer level with the project's rules applied; it is computed when needed, never stored.
4. **The vocabulary is the wiki's, and there is no schema.** Tropes are tagged by their TV Tropes names, as a weighted bag, with no facets, hierarchy or local definitions to maintain. Structure (clusters, similarity, comps) is computed at query time from co-occurrence, by the model: schema-on-read. The system never copies TV Tropes text; it uses names and links.
5. **Everything is a file.** State is Markdown in the writer's folder, readable and editable by hand, versioned by git. No database, no server, no build step, no dependencies. An agent session resumes from the files alone.
6. **Quarantine against anchoring.** The writer's own stalled premises never enter a generator's context, and no idea folder ever reads another. Both are documented anchoring failures in co-creative systems.
7. **Originality is checked at the premise level, cheaply.** A candidate's trope set is compared to each comp's; a near-match is flagged before the writer sees it. This is the only automated "derivativeness" check that is both affordable and legally meaningful, since tropes are unprotectable and a premise can only be too close to a specific work (see `copyright-and-trope-based-ideas.md`).
8. **Model-agnostic, role-based.** Instructions name roles (generator, tagger, checker) and a tier (strongest available for generation; cheapest adequate for tagging and checking), never a vendor's model ID. A user's agent configuration maps roles to models.

## 3. Layout

```
AGENTS.md, CLAUDE.md          the system's rules; CLAUDE.md holds only "@AGENTS.md"
README.md                     what it is, how to start, in five minutes
LICENSE                       MIT
docs/design.md                this document
docs/copyright-and-trope-based-ideas.md
docs/evaluation.md            how to test that the loop beats a bare chatbot prompt
_template/
  writer/                     skeleton of the writer-level folder
  idea/                       skeleton of one idea; copied, never run in place
writer/                       the writer's own taste, across all ideas; gitignored
ideas/                        the writer's ideas, one folder each; gitignored
```

The writer-level folder, copied once from `_template/writer/`:

```
writer/
  interview.md                the writer's own words about fiction in general; never edited by an agent
  profile.md                  weighted trope bag with evidence pointers; agent-written, writer-editable
  comps.md                    works the writer loves and hates, each tagged, each with the writer's one-line reaction
  own-ideas.md                quarantined: the writer's premises and stalled attempts
```

One idea folder, copied from `_template/idea/`:

```
ideas/<idea>/
  AGENTS.md, CLAUDE.md        the idea's rules, the idea named in the writer's words at the top
  interview.md                what the writer wants from this project, in their words
  rules.md                    project rules: tropes steered toward or away for this idea, with evidence
  comps.md                    comps specific to this project, tagged and reacted to
  signal.md                   append-only log of every reaction, verbatim, dated
  rounds/NN/ballot.md         ten premises with tags; the writer answers three questions under each
  rounds/NN/expand-<slug>.md  on demand: full pitch and stress test of one premise
```

`writer/` and `ideas/` are gitignored because a taste profile is personal and the public repository must never carry one. A writer who wants their taste and ideas versioned un-ignores both folders in a private fork or clone.

## 4. The loop

**Stage 0: Interview → `writer/interview.md` and `ideas/<idea>/interview.md`.** The writer-level interview runs once: what the writer has loved in fiction and why, what they hate on sight, what they wish existed, what they have tried and abandoned. The idea-level interview runs at the start of each project: what this book is for, what it must have, what it must not, which of the writer's usual tastes it should set aside. Both transcripts are saved in the writer's words. Elicitation by a model asking questions outperforms prompting and example-labeling in the published comparisons (GATE, ICLR 2025), and it costs the writer the least. The interview can be revisited at any time; additions are appended, never rewritten.

**Stage 1: Profile → `writer/profile.md` and `ideas/<idea>/rules.md`.** The tagger maps the writer-level interview to TV Tropes names, one line per trope: `TropeName | weight | evidence`, where weight is one of `love`, `like`, `avoid`, `kill` and evidence is a short quote from an interview or from `signal.md`. The idea-level interview becomes `rules.md` in the same format, holding only what differs for this project: `toward` and `away` entries that override or add to the writer level. The effective profile for a round is the writer level with the rules applied. Works the writer named are listed under the matching `comps.md` with their own tags. The writer reads the profile and corrects it by hand if they wish; the file is theirs. Anything the writer wrote is never paraphrased into the profile without an evidence pointer back to the words.

**Stage 2: Comps → `comps.md`.** The tagger proposes further works whose trope sets overlap the `love` cluster, from the model's own knowledge, and verifies each tag set against the work's TV Tropes page where one exists (a single page fetch, not a scrape). The writer reacts to each comp in one line: would I read this, and why or why not. A comp the profile predicts the writer would love and the writer rejects is the most informative event in the system: it means the profile lacks a tag that separates the two, and the agent mines the wiki for candidate tags and proposes them. Comps are also the reference set for the originality check.

**Stage 3: Ballot → `rounds/NN/ballot.md`.** The generator produces ten premises of at most one paragraph each in total, with no dialogue snippets, sample prose or scene fragments (they delight without changing a reaction, and they cost tokens), each followed by its trope tags. Composition rules enforce diversity against mode collapse: every premise combines `love` tags with one or two tags not yet in the profile; no two premises on a ballot share more than half their tags; at least two premises on each ballot deliberately subvert a `love` tag; no `kill` tag appears. The checker then drops any premise that breaks a rule, duplicates an earlier ballot or lands within a set overlap of a single comp, and the generator refills to ten. Under every premise the ballot carries three prompts the writer answers in prose, as long or short as they like: **Like** (what works), **Dislike** (what doesn't), **Missing** (what it needed). A blank answer is allowed. The writer may add a disposition word: `no`, `ingredient` (keep an element), `variants` (more like this), `write` (expand this).

**Stage 4: Signal → profile delta.** The agent copies each reaction verbatim into `signal.md`, then proposes changes as a diff: tags up-weighted, down-weighted, added or killed, each with its evidence quote. Each change is routed to a level: a reaction phrased about this project ("not for this book", "here I want") or about a premise's particulars goes to `rules.md`; a reaction phrased as general ("I never", "I always", "in anything") goes to `writer/profile.md`; when the agent cannot tell, the change goes to `rules.md` and the agent asks whether to promote it. A rule that recurs across two ideas is proposed for promotion to the writer level. The writer confirms or edits; the default is confirmation, and a writer who trusts the loop can set `auto` in their idea's `AGENTS.md` to apply deltas without asking. The next ballot is generated from the updated profile plus the `variants` and `ingredient` items.

**Stage 5: Expansion → `rounds/NN/expand-<slug>.md`, on demand only.** When a premise is marked `write`, the generator produces the full pitch (protagonist, want, obstacle, setting, stakes, shape of the ending, series shape if wanted) and a stress test: what breaks the premise at book length, what a hostile reader says, what the nearest comp does better. This is the one expensive step and it runs once per candidate, not per round.

**Stopping.** The loop ends when an expanded premise survives its stress test and the writer confirms they will write it. It also pauses, with a note to the writer, when two consecutive rounds change no weights (the profile has converged and the ballots are no longer teaching anything) or when every premise on a ballot is `no` (the profile is wrong somewhere, and the comps step is rerun).

## 5. Cost model

| Step | Calls | Tier | Frequency |
|---|---|---|---|
| Interview | conversation | any | once, plus additions |
| Profile build / update | 1 | cheap | per round |
| Comps proposal and verification | 1 + page fetches | cheap | at start and after a surprising rejection |
| Ballot generation | 1 | strongest | per round |
| Check and refill | 1 | cheap | per round |
| Expansion and stress test | 1–2 | strongest | per `write` only |

A round is two to three calls. The previous pipeline this replaces ran several strongest-tier generators, a multi-reviewer panel and a convergent pass per round; the new round costs roughly an order of magnitude less and can run as often as the writer reacts.

## 6. What makes it more than a chatbot prompt

A general chatbot with a good prompt reproduces the interview and a one-off list of premises, and costs the writer nothing extra. The system is worth running because it keeps what a chat session loses: a persistent, editable profile that accumulates across rounds and across ideas; a comps set that acts as both taste mirror and originality reference; an append-only record of every reaction; composition and diversity rules the writer never has to type; a quarantine the writer never has to remember; and a stop criterion. All of it is files, so it is inspectable and portable.

## 7. Alternatives rejected

- **A faceted schema over the tropes** (setting, protagonist, conflict engine, tone, …). Rejected for maintenance cost: someone has to define and police the facets, and the owner would be the writer. The folksonomy's emergent clusters carry the specificity instead, at query time. Recorded 2026-11-05 on the writer's position.
- **An adversarial review panel per round.** Rejected on the evidence that model judges rank originality poorly; retained only as a cheap rule-checker (Stage 3) and as the stress test in Stage 5.
- **A bulk trope dataset in the repository** (DBTropes, tropescraper, a TV Tropes scrape). Rejected: the public datasets are film-only and years old, TV Tropes text is CC BY-NC-SA with unverified scraping terms, and the model already knows the vocabulary; a per-work page fetch verifies what matters.
- **Numeric ratings or pairwise comparisons** for the ballot. Rejected: numbers lose the reasons, and the profile learns from reasons. Pairwise comparison (Bradley–Terry style) converges on ranking, which the writer does not need; the writer needs the tags behind the reaction.
- **A hosted web app with accounts.** Rejected for this version: it adds a server, a database, privacy obligations and cost, and the writer's agent already provides the interface. The file layout is designed so a hosted version could read the same files later.
- **A single `requirements.md` plus derived `constraints.md`.** Replaced by `interview.md` (words) plus `profile.md` and `rules.md` (tags with evidence pointers). The split between the writer's words and the agent's derivation stays; the derivation is now tags, not numbered constraints, because tags are what the generator and the comps step both consume.
- **A flat, per-idea profile.** Replaced by the two-level profile (writer, then project rules), because a writer's general taste should not be re-interviewed for every idea, and a project should be free to want a trope the writer usually avoids. Recorded 2026-11-05 on the writer's position.
- **Dialogue snippets and sample prose in a pitch.** Dropped on the writer's experience: they delighted but never changed a reaction, and they cost the most tokens of anything in a ballot. Recorded 2026-11-05.
- **Ranking the ballot.** Dropped for per-premise like / dislike / missing feedback, on the writer's position: the three questions produce the tags a ranking would hide. Recorded 2026-11-05.

## 8. Risks and mitigations

- **Mode collapse / homogenized premises.** All frontier models show low output diversity. Mitigation: the composition rules in Stage 3 (novel tags, maximum shared tags, mandatory subversions) and the duplicate check across all prior ballots.
- **Sycophancy toward the profile.** A generator conditioned on `love` tags over-fits them. Mitigation: mandatory subversions on every ballot and the comps rejection test, which catches a profile that predicts the writer's taste wrongly.
- **Trope overlap is a proxy for taste.** Two premises with the same tags can read as different books. Mitigation: the profile is weighted and evidenced, reactions are prose, and surprising rejections trigger tag mining; the proxy is corrected from evidence every round.
- **Preference drift.** Stated wants change as the writer sees candidates. This is expected, not a failure: `signal.md` is append-only and `profile.md` carries evidence dates, so drift is visible and the writer can revert a weight.
- **Tag hallucination.** The model may invent a trope name. Mitigation: the checker verifies that each tag resolves to an existing TV Tropes page (one fetch per unknown tag) and strikes any that does not.
- **Personal data in a public repository.** Mitigation: `ideas/` is gitignored by default and `README.md` says so in its first section.

## 9. Evaluation (to be written in `docs/evaluation.md`)

Two tests, cheap enough to run before any further build: (a) with a volunteer writer, does interview plus first ballot produce a premise the writer rates above what the same writer gets from a single good chatbot prompt; (b) how many rounds does it take a volunteer to mark a premise `write`. The target for (b) is a handful, not a dozen. Results go in the README as evidence, not marketing.

## Decisions taken by the writer, 2026-11-05

- **Name**: Tropestorm (brainstorming with tropes as the unit).
- **License**: MIT. Permissive and standard; it has no share-alike entanglement with the CC BY-NC-SA text the repository deliberately excludes.
- **Repository**: this folder, with `writer/`, `ideas/`, `research_notes/` and `reports/` gitignored so the public history never holds personal data. The repository is a list of Markdown documents, not a code project, so it does not live under the code-projects folder.
- **Feedback, not ranking**; **one paragraph per pitch, no dialogue snippets**; **two-level profile** (writer, then project rules). Each is recorded above with its reasoning.
