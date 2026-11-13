---
name: brand-voice-builder
description: Build or refine a brand-voice.md file through an adaptive interview process. Use this skill when the user wants to create or update their brand voice document, says "build my brand voice," "update my brand-voice.md," "refine my brand guidelines," "I need to fill out the brand voice file," or "help me define my voice." Also trigger when the user is setting up the pre-publish QA system for the first time and needs to complete their brand-voice.md before the other skills will work properly. Handles both from-scratch creation and refinement of an existing file equally well.
---

# Brand Voice Builder

Creates or refines a completed `brand-voice.md` through an adaptive interview. The output is a file the user can immediately upload to their Claude Project — not a worksheet, not a template with blanks.

## Step 1: Assess the Situation

First, determine which mode you're in:

**Scenario A — From Scratch**
User has no existing brand-voice.md, is new to the process, or is starting a new publication.

**Scenario B — Refinement**
User has an existing brand-voice.md and wants to update, expand, or improve it.

**How to detect:** Ask if they have one already, or check if they've pasted/uploaded one.

---

## Step 2: Choose Extraction Method

Don't default to a rigid interview. Read what the user brings and adapt:

| What they provide | Best approach |
|---|---|
| Nothing yet | Start with 3 open questions (see Interview Mode below) |
| A brain dump / long description | Extract from it, then fill gaps |
| Sample posts (good and bad) | Analyze them first, then confirm findings with user |
| Existing brand-voice.md | Gap analysis + targeted refinement questions |
| Mix of the above | Combine — extract first, interview for gaps |

Tell the user upfront which mode you're using and why. Example:
> "You've given me a few example posts — I'll analyze those first to extract your voice patterns, then ask a few targeted questions to fill in what I can't infer. That'll be faster than a full interview."

---

## Interview Mode (From Scratch)

If starting cold, don't dump a 20-question form. Run a progressive interview in 3 rounds. Each round builds on the last.

### Round 1: Foundation (3 questions)

Ask these together, clearly numbered:

1. **Publication basics:** What's the name of your publication, who is it for, and what's the one-sentence promise you make to your reader?

2. **Voice in your own words:** How would you describe your writing style to a ghostwriter you'd just hired? What should it sound like, and what would immediately signal "this isn't her"?

3. **Your best and worst:** Link or paste an example of a post you felt nailed your voice — and one where you felt it drifted or fell flat. (If you don't have examples handy, describe them.)

Wait for responses before continuing.

---

### Round 2: Specifics (targeted gaps)

Based on Round 1 answers, identify 3-5 gaps and ask only about those. Do not ask about things already answered. Possible questions to draw from:

- "You mentioned you want it to feel [X] — can you give me 2-3 phrases or sentences from your writing that you think best capture that?"
- "What words or phrases do you find yourself deleting when editing? The ones that sneak in but don't sound like you?"
- "Are there any British colloquialisms you use naturally that sometimes confuse your (presumably non-UK) readers?"
- "What topics or keyword areas does your publication cover? What would you want every post to be relevant to?"
- "What are your grammar non-negotiables? Oxford comma yes/no? Exclamation marks — never, rarely, or fine in moderation?"
- "How do you typically open a post? What's your hook style?"
- "How do you close? Do you always have a CTA? What does it usually look like?"
- "Describe your ideal reader's emotional journey through a post — where do they start and where do you want them to end up?"

---

### Round 3: Confirmation

Draft the brand-voice.md and present it to the user. Say:
> "Here's your brand-voice.md based on what you've shared. Read through it — does anything feel off, missing, or not quite right? I can adjust any section before we finalise."

Make requested edits, then proceed to Step 4.

---

## Analysis Mode (From Sample Posts)

If the user provides example posts:

1. Read all provided posts carefully.
2. Extract the following patterns:
   - **Sentence rhythm:** Average sentence length, use of short punchy sentences, variation patterns
   - **Vocabulary range:** Formal vs. casual, technical vs. accessible, any recurring phrases
   - **Structural habits:** How they open, how they close, how they use headers
   - **Emotional register:** What feeling does the writing create? What relationship does it assume with the reader?
   - **What to avoid:** If bad examples provided, extract what went wrong specifically

3. Present your findings:
> "Here's what I've inferred from your posts. Tell me if anything is off — I'd rather you correct my read than bake in a wrong assumption."

4. Fill remaining gaps with targeted questions (3-5 max).
5. Proceed to draft.

---

## Refinement Mode (Existing brand-voice.md)

If the user provides an existing file:

1. Read it completely.
2. Run a gap analysis against the full brand-voice.md schema (see Schema section below).
3. Note: sections that are blank, sections that are too vague to be actionable, and any internal contradictions.
4. Present findings:
> "Your brand-voice.md is [X]% complete. Here's what I'd recommend addressing: [list gaps and vague sections]. Want to work through those, or is there a specific section you want to focus on?"
5. Ask only targeted questions for identified gaps.
6. Produce an updated version of the full file.

---

## Schema: What brand-voice.md Must Contain

A complete file has all of these. Use this as your checklist before finalising.

**Required — Core Voice**
- [ ] Publication name and one-line positioning
- [ ] Target reader description
- [ ] Core promise to the reader
- [ ] 3 adjectives defining writing style
- [ ] What it sounds like (positive description)
- [ ] What it never sounds like (negative description)
- [ ] 2-3 signature writing moves

**Required — Language Rules**
- [ ] Banned words/phrases list (at minimum: common AI-generated terms)
- [ ] British colloquialisms to flag (at minimum: the standard list)
- [ ] Oxford comma rule (yes/no)
- [ ] Quote style (curly/smart vs. straight)
- [ ] Exclamation mark policy
- [ ] Header case style (sentence vs. title)

**Required — Structure**
- [ ] Mobile paragraph length rule
- [ ] Preferred paragraph patterns (1-3-1 / 1-2-1 etc.)
- [ ] Typical post architecture (opening style, section count, closing style)
- [ ] Emotional arc statement

**Required — SEO & Substack**
- [ ] 3-5 keyword pillars
- [ ] Email preview text standard (first 140 chars)
- [ ] Pull quote standard (count, length)
- [ ] CTA patterns

**Optional but valuable**
- [ ] Pocket Personalities / AI collaboration context
- [ ] Description of best-performing post
- [ ] Recurring content elements

---

## Step 4: Produce the Final File

Write a complete, filled-in `brand-voice.md` — not a template with placeholders, not a worksheet. Every section should contain real, usable content based on the interview and analysis.

Rules for the file:
- **No empty brackets.** If a section can't be filled from the interview, note "To be added" with a brief description of what goes there — never leave a silent blank.
- **Be specific.** "Warm and direct" is too vague. "Reads like a smart friend who respects your time — never talks down, never over-explains, gets to the point but doesn't skip the humanity" is usable.
- **Banned word lists should be real and reasonably complete.** Seed with the standard AI-generated word list plus anything the user mentioned. The user can add to it but shouldn't have to start from zero.
- **Preserve the user's voice in the voice description.** If they described their style in a particular way, use their phrasing — don't paraphrase it into blandness.

Use the brand-voice.md template structure from the pre-publish-qa skill system. Output the file in full.

---

## Step 5: Deliver and Instruct

Present the complete file content, then say:

> "This is your `brand-voice.md`. To activate it with the QA skills:
> 1. Copy this file (or download it)
> 2. Upload it to your Claude Project's knowledge base
> 3. The creative-qa-check, publication-proofreader, and pre-publish-qa skills will pick it up automatically
>
> Review it before uploading — especially the voice description and banned words sections. Those are the ones that most affect QA quality. You can run this skill again any time to refine it as your publication evolves."

---

## Tone & Process Notes

- This is a collaborative build, not a form-filling exercise. Stay conversational.
- Push back gently if answers are vague. "Warm and professional" helps no one. Ask for an example.
- If the user gives you a brain dump, extract aggressively and present back — don't make them repeat themselves in interview format.
- The file is only as good as the specifics. Generic adjectives produce generic QA. Your job is to get the specifics out of them.
