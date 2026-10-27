# AI Proposal Triage Design

**Candidate:** Osama Rizwan
**Reference:** MATACS-TH-2026-02
**Role:** Associate Software Developer

## Time spent

About 60 to 70 minutes.

## How I used AI tools

I used Claude to understand the brief and to draft the design files, and I reviewed and edited everything myself. Before the prompt was generated, I gave Claude my basic idea and the main points I wanted it to cover, and the extraction prompt was built from those. After that I added some rules of my own to the prompt.

I also disagreed with parts of Claude's first draft. It had a second AI step for scoring "team and approach", but that score is only a checklist of four items, so I asked for it to be removed and done by code, and the diagram was updated to match. I also asked for the first WRITEUP draft to be cut down because it was too long and included risks I did not need.

## Files in this repository

- [diagram/process-flow.png](./diagram/process-flow.png) - process flow from upload to ranked output, showing AI steps, rule-based steps and human checkpoints
- [WRITEUP.md](./WRITEUP.md) - answers to the five questions
- [PROMPTS.md](./PROMPTS.md) - the AI extraction prompt, as it would be sent
- [ranking.json](./ranking.json) - expected ranking of proposals A-E

## Assumptions

- The brief does not give scoring formulas, so the scoring bands are my own. The same rule is used for every proposal.
- C is flagged because of hidden instruction text, and D is flagged because it gives no completion time.