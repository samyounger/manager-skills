---
name: cognitive-clarity
description: Make answers and work documents easier to digest when the user requests cognitive clarity, dyslexia-friendly communication, less text, or a concise explanation. Apply to technical proposals, investigations, management updates, and reading-heavy summaries while preserving evidence and important caveats. Shapes communication; does not replace domain-specific work.
metadata:
  version: 1.2.0
---

# Cognitive Clarity

Help the reader understand the answer and decide what to do with minimal reading effort.
Treat these as this user's communication preferences, not a clinical intervention or universal rules for dyslexia. Use en-GB spelling. Optimise for understanding on the first read. Use enough words to make relationships explicit; fewer words do not necessarily mean less reading effort.

## Essential behaviour

1. **Answer first.** Open with the recommendation, result, or key finding. Include a material caveat alongside it. Do not begin with a recap or a plan unless the task needs a progress update.
2. **Keep the first layer small, without compressing the wording.** Usually use a short paragraph and up to three key points. Use short sentences and paragraphs, familiar words, descriptive headings when useful, and whitespace between ideas. Avoid repetitive conclusions, decorative formatting, and dense nested lists. Keep subjects close to their verbs. Prefer familiar word combinations, explicit connections, and literal wording over stacked nouns, invented verbs, or metaphors that need unpacking. Keep useful connecting words such as “that”, “because”, and “so”.
3. **Make decisions easy.** Recommend one option with its reason and main trade-off when the evidence supports a choice. If evidence is insufficient, state the missing fact and next check instead of manufacturing certainty. Use a small table only when it improves comparison.
4. **Preserve meaning.** Retain important risks, dependencies, dates, units, breaking changes, disagreements, and action owners. Explain unfamiliar acronyms on first use. Keep exact identifiers and commands where needed. Distinguish evidence, inference, and unknowns when material; do not add empty labels to every answer.
5. **Layer detail without withholding the deliverable.** For a requested proposal, report, or full procedure, provide the complete document with a short opening summary and clear sections. Put supporting detail later. For ordinary chat, omit optional depth until requested. Do not force the user through repeated prompts to obtain required work.
6. **Reduce interruptions.** Keep progress updates brief and limited to meaningful findings, decisions, or blockers. Ask only questions that affect the result, grouped when possible. Avoid ending every response with an offer or question.
7. **Adapt to the reader and destination.** Honour requested formats and repository conventions, including required nested acceptance criteria. Keep chat summaries separate from stakeholder-facing documents. Do not disclose the user's dyslexia in shared material unless requested.

For management tasks, first read `.agents/em-context.md` if available; otherwise read `.agents/skills/em-context/SKILL.md` if available. Do not block a general explanation or invent team context when these are absent. Use domain skills and verification appropriate to the task; this skill only controls communication.

## Read only the relevant supporting guide

- **Technical proposal, RFC, architecture decision, or options comparison:** [Technical decisions](references/technical-decisions.md).
- **Status update, management brief, incident, or investigation:** [Management and investigations](references/management-and-investigations.md).
- **Long source, explanation, workflow, listening-friendly text, or document layout:** [Reading and presentation](references/reading-and-presentation.md).

## Final check

Can the reader identify the answer, important caveat, and next action in seconds? Can each sentence be understood without rereading or reconstructing missing connections? Is optional detail separated from the main message? Are technical meaning and uncertainty intact? Remove repetition, not essential information.
