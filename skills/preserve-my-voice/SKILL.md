---
name: preserve-my-voice
description: Applies when the user asks for feedback on, a review of, or edits to their own written draft, including messages, emails, proposals and updates. Trigger phrases include "review my draft", "tidy this up", "does this read well?", "keep my voice" and "light edit". Does not apply to feedback about a person's performance or writing from scratch without a supplied draft.
metadata:
  version: 1.0.0
---

# Preserve My Voice

Make the smallest useful changes so the draft still sounds like the user. The supplied draft is the primary evidence of their voice.

## Before editing

For management drafts, read `.agents/em-context.md` if available, or `.agents/skills/em-context/SKILL.md` otherwise. Do not block editing or invent context if neither exists.

Read the whole draft and any explicit editing request. Notice its structure, tone, level of formality, phrasing, contractions and rhythm. If the user explicitly requests a substantial rewrite or a different tone, follow that request; otherwise default to a light edit.

## Editing rules

- Preserve the order of ideas, paragraph structure, headings and list format. Keep greetings, sign-offs and personal turns of phrase.
- Fix spelling, grammar, punctuation and wording that obstructs understanding. Use en-GB spelling, while preserving exact quotations and identifiers.
- Keep edits local. Do not add headings, convert prose into bullets, reorder arguments or replace everyday wording with corporate language merely to polish it.
- Preserve meaning, emphasis, uncertainty, ownership and commitments. Do not invent facts, deadlines, promises or stronger claims.
- Leave wording alone when it already works. A draft may need no changes. Keep optional structural improvements as brief suggestions after the draft.
- Apply other writing guidance, including `cognitive-clarity` and `written-communication`, within this light-edit scope. General advice to shorten or restructure is not a reason to override the user's voice.

## Factual and questionable claims

Check claims against the available evidence. Verify material claims using appropriate sources where needed, and distinguish a confirmed error from uncertainty or missing evidence. Never claim to have verified something that has not been checked.

Do not silently change factual claims or resolve ambiguities by guessing. Raise every identified factual error or questionable claim in a separate comment after the revised draft. Quote the relevant phrase, explain the concern briefly, and give a supported correction or the specific check needed. Link supporting sources when available. Make clear when wording retained from the draft is known to be incorrect. Suggestions about potentially misleading or unsupported wording belong here too.

## Response format

1. Put the complete, lightly edited draft first. Any factual concerns in the comments must be addressed before sending.
2. Add **Comments** afterwards only when there are factual concerns, ambiguities or material suggestions. Keep comments outside the draft. Do not add a generic all-clear or a routine change log.

If the user asks for feedback only, provide comments without an unsolicited rewrite. If they ask for feedback and a redraft, provide both in the order above.

## Final check

Compare with the original: does it still sound like the same person, with the same structure and intent? Undo unnecessary edits. Ensure all identified factual concerns appear after the draft.

## Verified example

A verification draft included “Hi folks”, “I reckon we're nearly there”,
“see whats still awkward?” and “12 of the 40 checks passed, so we're at 80%”.
The light edit kept the greeting, informal phrasing and paragraph order,
corrected “whats” to “what's”, and flagged the arithmetic in a comment after
the draft: 12 out of 40 is 30%, not 80%. The comment made clear that the
incorrect percentage had been retained pending correction.

## Related skills

- `cognitive-clarity` — clarity and accessible communication.
- `written-communication` — broader drafting and audience guidance.
