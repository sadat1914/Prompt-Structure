---
name: prompt-testing-evaluate-enhance
version: '"1.1.0"'
last_updated: 2026-04-24
status: production
changelog: added instructions to reformat submitted prompt as markdown with no changes
---

# Prompt Evaluation

## Description
Use this skill when the user asks for evaluation, review, audit, critique, or analysis of a SKILL.md file, system prompt, persona prompt, or instruction set intended for use in an LLM context. Triggers include phrases like "evaluate" "review" "audit".  Minimal prompts (one or two unstructured paragraphs) are valid input. If findings are sparse, 'None identified' is the correct output — do not pad findings to compensate.

## Instructions

When this skill is invoked, evaluate the pasted prompt against the eight criteria below. Return findings in the exact format specified. No preamble, no praise, no closing remarks.

### Evaluation Criteria

Assess the prompt for issues in these categories:

1. **Ambiguity** — Instructions open to multiple reasonable interpretations. Flag vague qualifiers ("appropriately," "when relevant," "as needed") without operational definition.
   *Example:* "Respond with appropriate detail" — "appropriate" is undefined.

2. **Contradictions** — Rules that directly conflict. Both cannot be followed simultaneously.
   *Example:* "Always cite sources" and "Never include citations in responses."

3. **Scope creep** — Instructions phrased so broadly they will fire on inputs outside the intended use case, or trigger descriptions that over-match.
   *Example:* A code-review skill with trigger "review this" will fire on essays, emails, and resumes.

4. **Missing constraints** — Implicit assumptions the author relied on but did not state. Includes undefined terms, unspecified output formats, unhandled input types, and absent failure modes.
   *Example:* "Return the data as a table" without specifying columns, sort order, or what to do if the data is empty.

5. **Instruction priority conflicts** — Competing rules where both can be valid but no tiebreaker is specified. Distinct from contradictions: contradictions cannot both be true; priority conflicts can both be true but pull in opposite directions.
   *Example:* "Be concise" and "Include all relevant context" — both reasonable, no rule for which wins when they collide.

6. **Token efficiency** — Redundant phrasing, repeated instructions, verbose constructions, or filler that adds length without changing model behavior.
   *Example:* "Please make sure to always remember to" → "Always."

7. **Edge case exposure** — Specific inputs or scenarios that would produce unexpected, broken, or undefined behavior given the current instructions.
   *Example:* A translation skill with no rule for inputs that are already in the target language.

8. **Persona/role drift risk** — Instructions likely to erode over long conversations: personality traits stated once without reinforcement, behavioral rules embedded in flavor text, role definitions that conflict with default model behavior without explicit override language.
   *Example:* "You are a gruff pirate" stated once at the top — tone will regress to baseline within a few turns.

### Analysis Method

Work through the prompt sequentially. For each issue found, cite the exact flagged text — do not paraphrase. If a single passage has multiple issues, list each separately. If a category has no findings, write "None identified" under that category heading. Do not invent issues to fill categories.

Distinguish real issues from stylistic preferences. A concise instruction is not "missing context" just because it could be longer. A strict rule is not a "contradiction" just because it limits flexibility.

If the pasted content contains non-instruction sections (code blocks, data samples, examples, tables), evaluate only the instruction text. Note any skipped sections in one line before the findings.

### Output Format

Return the assessment as Markdown using this exact structure:

---

## Prompt Evaluation

[One line only if applicable: "Skipped non-instruction sections: <brief note>"]

### Ambiguity
- **Flagged text:** "<exact quote>"
  **Problem:**
  **Fix:**

### Contradictions
- **Flagged text:** "<exact quote>" vs. "<exact quote>"
  **Problem:**
  **Fix:**

### Scope Creep
- **Flagged text:** "<exact quote>"
  **Problem:**
  **Fix:**

### Missing Constraints
- **Implicit assumption:**
  **Problem:**
  **Fix:**

### Instruction Priority Conflicts
- **Competing rules:** "<exact quote>" and "<exact quote>"
  **Problem:**
  **Fix:**

### Token Efficiency
- **Flagged text:** "<exact quote>"
  **Problem:**
  **Fix:**

### Edge Case Exposure
- **Input scenario:**
  **Expected failure:**
  **Fix:**

### Persona/Role Drift Risk
- **Flagged text:** "<exact quote>"
  **Problem:**
  **Fix:**

### Overall Assessment
**Score:** [0–100]
**Rating:** [Production-ready | Minor issues | Major issues | Needs rewrite]
**Primary reason:** [One sentence]

## Reformatted Prompt
[Submitted prompt reformatted as clean Markdown — no content changes]

### Rules

- Every issue must include the required fields (flagged text / problem / fix). No freeform commentary.
- Quote exactly from the source prompt. Do not paraphrase flagged text.
- Fixes must be concrete and directly applicable — not "consider adding more detail."
- Fixes are scoped to the flagged passage only: a single sentence or clause replacement, not a restructure of the surrounding section. If an issue cannot be resolved without broader rewriting, state that in the fix line and stop there.
- If the pasted content is not a SKILL file or system/persona prompt intended for LLM use, say so in one sentence and stop.
- Do not rewrite the entire prompt unless explicitly asked. The skill diagnoses; it does not reauthor.
- If no issues exist in any category, the response is eight headings each reading "None identified." This is a valid output.
- Minimal prompts (one or two unstructured paragraphs) are valid input. If findings are sparse, 'None identified' is the correct output — do not pad findings to compensate.
- Append a **Overall Assessment** section after the eight criteria. Rate overall prompt quality as: **Production-ready**, **Minor issues**, **Major issues**, or **Needs rewrite**. Include one sentence stating the primary reason for the rating. 
 - After completing the evaluation, reformat the submitted prompt as clean Markdown without altering any words, meaning, or instruction intent. Present it under a final section headed `## Reformatted Prompt`.
