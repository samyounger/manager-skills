---
name: preserve-my-voice
description: Applies when the user asks for feedback on or edits to their writing, or for new messages, emails, proposals, updates and other content written on their behalf in their voice. Trigger phrases include "review my draft", "tidy this up", "keep my voice", "light edit", "draft a message" and "write this as me". Does not apply merely because a task concerns another person's performance or asks for general advice.
metadata:
  version: 1.2.0
---

# Preserve My Voice

Preserve the user's voice when editing their words or drafting on their behalf. Make the smallest useful changes to an existing draft; use the voice reference for new writing.

## Before writing

For management writing, read `.agents/em-context.md` if available, otherwise `.agents/skills/em-context/SKILL.md`. Do not block or invent context if neither exists. Supporting skills must not introduce profile setup or file writes into a bounded writing task unless the user requests them.

Read the [voice profile and examples](references/voice-profile.md) before editing or drafting. Explicit current instructions take priority, followed by the user's stated preferences, the supplied original draft, then the reference profile. The profile supplies defaults for new writing; adapt it to another user's own voice. Do not impose this voice on quotations, another person's writing or ordinary assistant explanations.

Choose the requested mode:

- **Feedback only:** provide comments without rewriting.
- **Light edit:** retain the draft's structure, tone and personal phrasing.
- **New draft:** compose in the user's voice using the audience, purpose and supplied facts.
- **Substantial rewrite or different tone:** follow the explicit request while preserving meaning and factual accuracy.

## Editing and drafting

- Follow the reference profile for spelling, greetings and recommendation wording unless the user or their original draft establishes a different preference. Preserve exact quotations and identifiers.
- For light edits, preserve idea order, paragraphs, headings, lists, greetings and sign-offs unless an explicit preference requires a local correction. Do not restructure merely to polish.
- Fix errors and wording that obstructs understanding. Keep concrete examples, personal warmth, useful reasoning and varied sentence lengths. Leave wording that already works alone.
- Preserve uncertainty, emphasis, ownership, commitments and the force of requests. Do not turn a suggestion into a command, a condition into a promise or an expectation into an optional invitation.
- For new drafts, use a natural structure suited to the audience. Do not invent facts, personal experiences, praise, deadlines or commitments.
- Apply `cognitive-clarity` and `written-communication` within the requested mode. Clarity guidance does not authorise a substantial rewrite of an existing draft.

## Facts and response format

Check material claims using appropriate evidence. Never claim verification that was not performed. For edits, do not silently change factual claims or guess at ambiguities. Put the complete draft first, then **Comments** only for factual concerns, ambiguities or material suggestions. Quote each questionable phrase and explain the supported correction or check needed. Explicitly identify any known error retained in the draft; it needs resolution before sending.

For new writing, omit unsupported claims or flag essential missing information outside the draft. Do not add routine change logs or an all-clear. Feedback-only requests receive comments only.

## Final check

Does this sound like the user addressing this audience? Check the reference, preserve the intended certainty and request strength, undo unnecessary edits, and keep assistant commentary outside the authored content.
