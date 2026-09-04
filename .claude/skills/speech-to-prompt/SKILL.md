---
name: speech-to-prompt
description: Use when the user pastes rambling, spoken-style text (a voice transcript, stream-of-consciousness dictation, filler words) and wants it converted into clean, structured prompt text for an AI tool.
---

# Speech to Prompt

Converts rough, spoken-style input (voice-to-text transcripts, rambling dictation, run-on thoughts with filler words and false starts) into clean, well-structured prompt text ready to paste into an AI tool. Triggers automatically whenever the user pastes this kind of raw, unstructured text — no need for them to ask each time.

## Step 1: Detect the target type

Infer what kind of prompt this is meant to become, from context clues in the rambling text itself:

- **Coding / dev task** — mentions of code, a repo, a function, a bug, a feature, a file, an error, a language/framework.
- **AI image or video generation** — mentions of a visual scene, style, composition, mood, art direction, camera/lens language, or references to tools like Midjourney/DALL-E/Sora/etc.
- **Task/action prompt** (e.g. calendar event, email, reminder, form fill) — the rambling describes something to be created or done in a specific tool with defined fields (title, date, recipient, etc).
- **General AI assistant prompt** — writing, research, analysis, planning, or anything that doesn't clearly fit the above.

## Step 2: Clean the raw text

- Strip filler words and verbal tics ("um", "like", "you know", "basically", "I guess").
- Remove false starts, repetitions, and self-corrections — keep only the final intended statement when the user corrects themselves mid-thought.
- Do NOT invent new requirements, details, or constraints the user didn't say. Only reorganize and clarify what's there.
- Preserve every specific detail: names, numbers, technical terms, file/function names, exact wording of requirements.

## Step 3: Clarification loop

Before restructuring, check the cleaned text against the essential slots for its target type. Essential slots are the pieces of information the destination prompt genuinely can't work without or would otherwise be guessed.

Typical slots by type:
- **Coding task**: the specific goal/outcome, which file or function is affected.
- **Image/video generation**: the core subject; style is optional but worth confirming if totally absent.
- **Task/action prompt** (e.g. calendar event): item type, title text, description/notes text (kept separate from title), destination (which calendar, list, inbox, etc), date/time if applicable, and any other field the target tool requires.
- **General assistant prompt**: the core task/goal, and the output format if the request implies one exists but doesn't state it.

Process:
1. Identify which essential slots are missing or ambiguous in what the user said.
2. If none are missing, skip this step and go to Step 4.
3. If any are missing, ask about them **one question at a time**, in short plain language — don't dump a checklist. Use what's already stated to avoid asking about anything already answered.
4. After each answer, re-check: is anything essential still missing or unclear? If yes, ask the next question. If no, move on.
5. Keep the loop tight — don't ask about optional or stylistic details (tone, minor phrasing) unless the user brings them up. Only loop on things the destination prompt cannot function without.
6. If the user says to just proceed, or gives a "doesn't matter" / "you decide" answer, stop looping and use reasonable defaults for whatever's left, noting the assumption in the final output.

## Step 4: Restructure by type

**Coding tasks**: lead with the goal/outcome, then relevant context (files, current behavior), then specific requirements or constraints, then edge cases or acceptance criteria if mentioned. Plain prose unless the original content is naturally multi-part enough to warrant short structure.

**Image/video generation**: lead with the subject, then style/medium, then composition/framing, then mood/lighting/color, then any technical parameters (aspect ratio, camera details) the user mentioned. Dense, descriptive phrasing rather than full sentences is fine here since that's the convention for these prompts.

**Task/action prompts**: structure as labeled fields matching what the destination tool needs (e.g. Title / Description / Calendar / Date & Time / Reminders), each filled from what the user said or clarified.

**General assistant prompts**: lead with the task/goal, then necessary context, then constraints or preferences, then desired output format/length if mentioned.

## Step 5: Deliver

Output the cleaned prompt in a plain code block so it's trivially copyable. Keep it as tight as the source material supports — don't pad with headers, bullets, or boilerplate that wasn't implied by what the user said. If the source was short, the output should be short too.

If something essential is missing (e.g., a coding task with no clear goal stated), ask for just that one missing piece rather than guessing and padding it out.
