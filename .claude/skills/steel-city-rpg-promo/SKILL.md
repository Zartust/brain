---
name: steel-city-rpg-promo
description: >-
  Orchestration skill for Steel City RPG event promotion. Use this skill when
  Zack wants to run a full promotional push for an upcoming event across
  multiple platforms in one session, or when brand voice consistency needs to
  be enforced across all promo copy. Triggers include: run my promo push,
  generate all my posts for Game Day, help me promote this event everywhere,
  or any request that involves writing for more than one platform at once.
  Also triggers when brand voice or tone consistency is a concern across
  platforms. Coordinates community-event-promotion and brand-voice-builder
  skills together.
---

# Steel City RPG Promo Orchestrator

This skill coordinates a full promotional push for a Steel City RPG event,
ensuring brand voice consistency across all platforms and copy types.

It runs two sub-skills in sequence:
1. **brand-voice-builder** — load or confirm the Steel City RPG voice
2. **community-event-promotion** — generate platform-specific posts

---

## Step 1: Load Brand Voice

Before writing any copy, check whether a Steel City RPG brand voice reference
is available in the project context. Look for a `brand-voice.md` or equivalent
uploaded to the project.

**If brand voice file exists:**
> Confirm key traits with Zack before proceeding. Example:
> "I've got your brand voice loaded — warm, welcoming, community-first,
> Pittsburgh-proud without forcing it. Still accurate? Anything changed?"

**If no brand voice file exists:**
> Prompt Zack to build one using the brand-voice-builder skill, but adapt
> the interview for a community event brand (not a Substack newsletter).
> Key differences for Steel City RPG brand voice vs. Substack:
> - Skip: SEO keyword pillars, email preview text, pull quote counts
> - Add: how the community should feel to a nervous first-timer, how GMs
>   should be talked about, Pittsburgh identity approach, how to handle
>   system-agnostic language, tone difference between hype and authenticity

### Steel City RPG Core Voice (built-in reference until brand-voice.md exists)

Use these as defaults when no file is present:

**Personality:** Warm friend extending an invitation, not a promoter running
an event. Welcoming but never condescending. Enthusiastic but never hype-y.

**Pittsburgh:** Local pride is a quiet undercurrent, not a flag to wave.
Lawrence Hall, Lawrenceville, Butler Street — let the specifics do the work.

**GMs:** Collaborators and community members, not service providers. Celebrate
them. Make running games sound appealing, not obligatory.

**New players:** The lifeblood of growth. Never make them feel like outsiders.
Pre-gens, beginner-friendly, no experience needed — always true, always said.

**System diversity:** Proudly system-agnostic. Never imply D&D is the default.
Name systems specifically when known; use "various systems" when not.

**What it never sounds like:** Corporate event listing, gatekeeping, hype for
hype's sake, jargon-heavy, D&D-only, long-term-commitment-required.

**Register examples:**
- "No experience needed — just bring yourself and a willingness to go on an adventure."
- "Looking for GMs who want to share their favorite system with a room full of enthusiastic players."
- "Already knee-deep in a campaign and can't commit to another one? That's exactly what Game Days are for."

---

## Step 2: Inventory the Platforms

Ask Zack which platforms/communities need posts this push, or extract from
the conversation if already stated. Build a checklist:

```
Platforms this push:
[ ] Discord — [server name / channel / warmth level]
[ ] Reddit — [subreddit / post type / thread vs standalone]
[ ] Heylo — [group type / warmth level]
[ ] Other — [specify]
```

For each platform, confirm:
- Standing in that community (active / permitted / partner / cold)
- Whether a previous post exists to match voice to
- Any known rules (r/lfg requires system tags, r/pittsburgh uses weekly thread, etc.)

---

## Step 3: Fetch Event Details

Run web_fetch on the Meetup event URL before writing anything. Never rely on
memory — event details (game list, times, descriptions) change.

Extract: event number, date, time, venue, address, neighborhood, confirmed
systems/games, Discord link, any special copy details.

---

## Step 4: Generate Posts in Platform Order

Work through the platform checklist. For each one, apply the
**community-event-promotion** skill logic:
- Match tone to warmth level
- Match length to platform/channel type
- Apply brand voice consistency check before finalising

**Recommended order:**
1. Warmest communities first (sister Discord, LGS partner) — these are easiest
   to write and set the tone for the session
2. Reddit posts — require most platform-specific formatting attention
3. Cold or permission-only communities last — most careful framing needed

---

## Step 5: Brand Voice Consistency Check

Before presenting any batch of posts, do a quick internal check:

- [ ] Does every post sound like the same organisation?
- [ ] Is beginner-friendly language present where the audience may include newcomers?
- [ ] Is GM recruitment mentioned where the audience skews experienced?
- [ ] Is Pittsburgh identity present but not forced?
- [ ] Does any post sound like a corporate event listing or generic promo?
- [ ] Is D&D implied as default anywhere it shouldn't be?
- [ ] Is the tone appropriate to the warmth level of that community?

If anything fails, revise before presenting.

---

## Step 6: Present and Batch

Present all posts together with clear platform labels. Format as:

```
## [Platform — Channel/Context]
[Post copy]
---
```

After presenting, note:
- Any posts being held (e.g. Alien RPG r/lfg post pending game confirmation)
- Any platforms requiring mod permission before posting
- Any image/embed decisions (Discord link-only vs. PNG attached)

---

## Integration Notes

**brand-voice-builder**: Run first if no voice reference exists, or if Zack
wants to formally document Steel City RPG's voice. Adapt the interview away
from Substack-specific fields toward community event brand fields.

**community-event-promotion**: This orchestrator calls on its platform logic,
tone matrix, template library, and pre-post checklist. Don't duplicate that
logic here — reference it.

**Future automation hook**: When Zack's content automation workflow is set up,
the per-game r/lfg posts (one per confirmed system) should plug into this
orchestration flow as a batch step after the GM submission deadline.
