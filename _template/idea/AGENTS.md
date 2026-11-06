# <This idea's name, in the writer's own words>

This folder is one idea. The root `AGENTS.md` of the Tropestorm repository governs every stage of the loop; this file holds only what is specific to this idea and does not restate the root rules.

## Settings

- `auto: off`

`auto: off` means the agent shows each profile delta to the writer and applies it after confirmation. The writer sets `auto: on` to let the agent apply deltas without asking and report the diff afterward.

## Files

- `interview.md`: what the writer wants from this project, in the writer's words. The agent appends and never rewrites.
- `rules.md`: tropes steered toward or away from for this idea, layered on `writer/profile.md`.
- `comps.md`: comps specific to this project, tagged and reacted to.
- `signal.md`: append-only log of every reaction, verbatim and dated.
- `rounds/NN/ballot.md`: the ten premises of round NN, with the three prompts under each.
- `rounds/NN/expand-<slug>.md`: the pitch and stress test of one premise, created only on request.

## Isolation

This idea never reads another idea's folder. A generator or checker run for this idea receives `writer/profile.md`, `writer/comps.md`, this folder's `rules.md`, `comps.md` and `signal.md`, and every earlier ballot of this idea, and nothing else. It never receives `writer/own-ideas.md` or any file under another `ideas/<idea>/`.
