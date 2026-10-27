# WRITEUP.md

Reference: MATACS-TH-2026-02

## Assumptions

- The brief does not give scoring formulas, so the point bands are my own. I use the same rule for every proposal.
- C is flagged because of hidden instruction text, and D is flagged because it gives no completion time. I do not guess missing values.

## 1. AI vs. rules

AI does one job only: it reads each proposal and pulls out the facts (licence, price, weeks, warranty, team details) as JSON, with a short quote for each fact.

Everything else is plain code: the pass/fail checks, the score for each criterion, the weighted total and the sorting.

I drew the line here because AI is good at reading messy text, but if it does the scoring, the same proposal can get a different score on a different day. Code gives the same answer every time and is easy to explain. "Team and approach" is just a checklist of four items, so code handles that too.

## 2. Consistency and explainability

- The AI runs at temperature 0 and must return a fixed JSON format. If the JSON is broken, the code retries once and then flags the proposal.
- The extracted facts are saved, and ranking is calculated from the saved facts with fixed rules. So ranking the same proposals twice gives the same result.
- If extraction is run again, the wording may change slightly. In that case I keep the saved facts and show the reviewer the difference instead of replacing them.
- Code checks that every quote really appears in the proposal text. If it does not, the fact is flagged.
- For each proposal the reviewer sees: the criterion, the fact found, the quote as proof, the points and the weight.

## 3. Human in the loop

A person is involved at three points:

1. The reviewer checks every flagged or ineligible proposal (B, C and D in the sample set). They confirm or override, and must write a reason.
2. The reviewer can correct a fact that was extracted wrongly, with a reason.
3. A human makes the final award decision. The tool only gives a ranked list with reasons.

The audit log keeps: who uploaded what and when, the criteria version, the prompt version, the model, the AI input and output, all scores and flags, every override with its reason, and the final decision.

## 4. Changing criteria

Criteria are saved in versions. An edit creates a new version and the old one is never changed.

- Every score is linked to the version that produced it, so old results stay correct.
- The reviewer sees a notice that the scores use an older version, and can click re-score.
- A weight change needs no new AI call, because the facts are saved and scoring is code. Only a brand new criterion needs extraction to run again.

## 5. Risks

- **Wrong extraction:** every fact comes with a quote that code checks, and the reviewer can fix it.
- **Made-up facts:** the prompt uses null when something is missing or vague, and facts without a quote are flagged.
- **Manipulation:** proposal text is treated as data, not instructions. The AI only extracts and cannot change scores or ranking, so the hidden text in C has no effect. It is reported and flagged for a human.
- **Bias:** scores come only from the admin's criteria and the extracted facts. The company name is not used, and every score is visible to the reviewer.
- **Cost:** one AI call per proposal, results are saved, and re-scoring uses no AI.
- **Missing information:** a missing key fact (like D's timeline) becomes null and a flag. The tool never guesses.