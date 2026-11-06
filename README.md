# Tropestorm

Tropestorm is a brainstorming tool to help writers come up with original story ideas they'd be excited about writing. Originally created for fanfiction writers who want to expand into writing original fiction but can't come up with a story of their own that excites them as much as fanfiction does, Tropestorm helps writers brainstorm by interviewing them about media that already exists, asking them what they like, don't like, and feel is missing from each. Along the way, writers share what they do and don't like in stories in general. The AI then decomposes these into tropes, named as the [TV Tropes](https://tvtropes.org) wiki names them, and uses these tropes as the basis for generating and pitching new ideas to the writer. The same trope profile also identifies comps: the published works closest to what you want to write.

## How it works

Tropestorm is a folder of Markdown files and agent instructions that you run inside your own coding agent (any agent that reads `AGENTS.md`); you need no programming. Each round is ten premises of at most a paragraph each. Under each premise you answer three questions in prose: what you like, what you dislike and what is missing. The profile updates and the next ten premises land closer. When you say "I'd write this," the agent expands one premise into a pitch and stress-tests it. Tropestorm writes no prose for your book, keeps a profile you can read and edit by hand, and stops when you have a premise you want to write. You only give feedback; the agent runs everything else.
## Five-minute start

1. Clone this repository and open your coding agent in the folder.
2. Ask the agent to run the writer interview. It copies `_template/writer/` to `writer/` and asks about the fiction you love, hate on sight, wish existed and have abandoned.
3. Copy `_template/idea/` to `ideas/<kebab-name>/` (the agent does this on request), naming the idea in your own words.
4. Ask the agent to run the idea interview, which records what this particular book is for, what it must have and what it must not.
5. Ask for round 1. Open `ideas/<kebab-name>/rounds/01/ballot.md`, answer Like, Dislike and Missing under each premise, optionally add a disposition word (`no`, `ingredient`, `variants` or `write`), and tell the agent you are done. Repeat.

## Your taste stays on your machine

A taste profile is personal, so `writer/` and `ideas/` are listed in `.gitignore`, along with `research_notes/` and `reports/`. The public repository never carries your profile, your reactions or your ideas. To version your taste and ideas, work in a private fork or a private clone: make the repository private first, then delete the `/writer/` and `/ideas/` lines from `.gitignore`. Nothing else changes.

## Cost per round

A round is two to three model calls: one cheap call to update the profile, one call to the strongest available model to generate the ballot, and one cheap call to check it and refill failures. Comps need one cheap call plus one page fetch per work, at the start and after a surprising rejection. Expansion and stress test cost one to two strongest-model calls and run only on a premise you mark `write`. Because a round is this cheap, you can run several a day.

## How it differs from asking a chatbot

A chatbot with a good prompt interviews you once, hands you a list of premises, and starts from zero the next time you open it. Tropestorm keeps everything you tell it, so every reaction makes the next round better and the learning carries from one book idea to the next: you never explain your taste twice. What it has recorded about you sits in a plain-text file you can read and correct, so when it gets you wrong you fix it in a sentence instead of arguing with a chat window. The comps it finds tell you two things a chatbot does not: what you actually want to read, and when a premise lands too close to a book that already exists. It keeps the stalled premises you have circled for years out of the generator's view, so you get ideas that are new to you instead of rearrangements of the one you are stuck on. Every ballot comes out varied by rule, not by luck, so you are not reading ten versions of the same story. It stops when a premise survives its stress test and you say you will write it, instead of generating forever. And all of it lives as files in a folder you own, so nothing is locked inside anyone's app.

## Evidence

No results exist yet. `docs/evaluation.md` defines two tests: whether the interview plus first ballot beats a single good chatbot prompt, and how many rounds a writer takes to mark a premise `write`. Results will be reported here with the raw numbers.

## Documents

- [`docs/design.md`](docs/design.md): the specification, the decisions behind it and the alternatives rejected.
- [`docs/copyright-and-trope-based-ideas.md`](docs/copyright-and-trope-based-ideas.md): how copyright applies to trope-based premises, with sources. It is not legal advice.
- [`docs/evaluation.md`](docs/evaluation.md): the two tests that measure whether the loop works.
- [`AGENTS.md`](AGENTS.md): the rules your coding agent follows.

## License

MIT. See [`LICENSE`](LICENSE).
