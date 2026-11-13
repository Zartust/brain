---
name: game-day-content-generator
description: >-
  Full event content package generator for Steel City RPG Game Days. Use this
  skill when Zack has confirmed GMs and games and wants to generate all
  publishable content for an event in one pass. Triggers include: "generate
  everything for Game Day", "full content package", "give me all the posts",
  "generate the listings and spotlights", or any time GM submission data is
  present and multiple content types are needed at once. Orchestrates
  game-listing-generator and community-event-promotion skills in sequence.
  Output covers: Meetup game listings, poster hooks, bullet tags, Discord game
  spotlight posts, Meetup group discussion post, and venue Discord reply post.
---

# Game Day Content Generator

Orchestrates a full content package for a Steel City RPG Game Day from raw
GM submission data. Runs game-listing-generator and community-event-promotion
in sequence and produces all publishable outputs in a single pass.

---

## What This Produces

For each confirmed game:
1. **Meetup listing description** — full body copy for the individual game listing
2. **Poster hook** — 2-line copy for the Canva 4:5 Instagram template
3. **Bullet tags** — 3–4 descriptors for the poster tag row (`・` separated)
4. **Discord game spotlight post** — pre-event GM recognition + RSVP driver

For the event as a whole:
5. **Meetup group discussion post** — open seats / event announcement post
6. **Venue Discord reply post** — short reminder post for the venue's server

---

## Step 1: Gather Required Inputs

Before generating anything, confirm the following are present. If any are
missing, ask before proceeding — do not invent or assume.

### Per-game (required for each GM submission):
- [ ] Game title (if distinct from system)
- [ ] System / engine name
- [ ] GM name and Discord handle
- [ ] GM's pitch or description
- [ ] Individual Meetup listing URL (needed for spotlight and group post links)

### Per-event (required once):
- [ ] Event number (Game Day #)
- [ ] Date and time (day, date, start time)
- [ ] Venue name and neighborhood
- [ ] Main Meetup event URL
- [ ] Current RSVP count (for group discussion post framing)
- [ ] Day-of floor lead confirmed? (name and yes/no)

### Optional but useful:
- Terrain or special setup available at venue
- Any content notes or safety flags from GMs
- Whether this is the venue's first Game Day (affects venue Discord tone)

---

## Step 2: Run game-listing-generator for Each Game

Process each GM submission through the full game-listing-generator skill.
For each game, produce in order:

```
## [GAME TITLE — SYSTEM]

**POSTER HOOK**
[Line 1]
[Line 2]

**BULLET TAGS**
[Tag 1] ・ [Tag 2] ・ [Tag 3]

**MEETUP DESCRIPTION**
[Full listing copy]
```

Apply all game-listing-generator rules:
- Never pass through GM copy verbatim — always rewrite into Steel City RPG voice
- Strip all GM operational notes
- No GM name or handle in any listing output
- Tone detection from genre/system
- Fallback logic for missing fields
- Closer always ends: "Check out other events happening for RPG Game Day"
- Closer lead-in is game-specific, not generic

Separate each game with a horizontal rule (`---`).

---

## Step 3: Generate Discord Game Spotlight Posts

For each game, produce a Discord spotlight post using the template from
community-event-promotion. One post per GM. These are separate from the
Meetup listing outputs — different medium, different purpose.

### Template:
```
[HOOK LINE — atmospheric, adapted from listing hook]

**[GM HANDLE]** is running **[GAME TITLE]** this Sunday — [tight premise sentence].
[Key sell sentence: terrain, system angle, tone, unique mechanic — omit if nothing distinct.]
[THANK-YOU LINE — vary per post, never repeat same phrase within one event]

No experience needed, pre-gens provided. Grab a seat: [individual game Meetup listing URL]
```

### Thank-You Phrase Library (rotate — no repeats within same event):
- Big thanks to [name] for bringing this one.
- Thanks to [name] for making this one happen.
- Grateful to have [name] at the table for this one.
- [Name] is the reason this one exists — thank you.
- Huge shoutout to [name] for running this.
- Thank you to [name] for putting this together.
- Couldn't do this without [name] — thanks for running it.

### Rules (enforced per community-event-promotion skill):
- Hook line pulled or adapted from the listing hook — never verbatim, never flat
- GM handle bolded; game title bolded
- "this Sunday" phrasing — replace with specific date if event is more than
  5 days out at time of posting
- Thank-you line required; vary phrasing across all spotlights in the same event
- Individual listing URL only — not the main event URL
- No emojis, no hashtags
- No GM bio or self-assessment language
- Accessibility line exact phrasing every time: "No experience needed, pre-gens provided."

Present all spotlight posts together under a clear header after the listings.

---

## Step 4: Generate Event-Level Posts

### 4a: Meetup Group Discussion Post

**Purpose:** Drive RSVPs from the broader Meetup community. Surfaces open
seats across all tables. Posted in the Steel City RPG Meetup group discussion
board.

**Structure:**
- Opening: name the systems/games briefly — variety is the hook, not individual game pitches
- Framing: seats still available, event is this Sunday (or specific date)
- Accessibility line: no experience needed, pre-gens at every table
- Game list: bullet list of game titles with individual listing links
- Main event URL as the catch-all closer

**Tone:** Calm, inviting, warm urgency. Not desperate. Not hype-y. The
framing is "here's what's happening, come join" not "act now before it's gone."

**Length:** 4–6 sentences + link list. No headers needed.

**Never include:** RSVP counts, table counts framed as scarcity, anything
that signals anxiety about attendance.

### 4b: Venue Discord Reply Post

**Purpose:** Short reminder in the venue's Discord server, posted as a reply
to the original announcement thread. Assumes a previous post exists.

**Structure:**
- Acknowledge seats are still open (don't re-announce from scratch)
- Name the games in one breath — variety signal
- One-line accessibility note
- Main event URL

**Tone:** Casual, brief, friendly. This is a reply, not a new announcement.
3–4 sentences max.

**Never include:** Apologetic framing, urgency language, anything that sounds
like a second pitch.

---

## Step 5: Quality Check Before Presenting

Run through this checklist internally before presenting any output:

### Per-game outputs:
- [ ] Tone register consistent across listing, hook, and tags for each game?
- [ ] No GM names or handles in any listing or poster copy?
- [ ] No invented specifics not in the submission?
- [ ] GM pitch rewritten — not passed through verbatim?
- [ ] GM operational notes stripped from all listings?
- [ ] Accessibility line present in every listing?
- [ ] Closer present in every listing, says "events" not "games"?
- [ ] Poster hook exactly 2 lines, ≤~40 characters each?
- [ ] Bullet tags ≤3 words each, `・` separator?
- [ ] Nothing implies D&D as default?
- [ ] Zero emojis anywhere?

### Per-spotlight outputs:
- [ ] Hook line adapted — not a direct paste from listing?
- [ ] GM handle bolded, game title bolded?
- [ ] Thank-you line present and varied from other spotlights in same event?
- [ ] Individual listing URL (not main event URL)?
- [ ] Accessibility line exact phrasing?
- [ ] No emojis, no hashtags?

### Event-level outputs:
- [ ] Group discussion post links to individual listings, not just main event?
- [ ] Venue reply reads as a reply, not a re-announcement?
- [ ] No RSVP counts or scarcity language in either post?
- [ ] Tone calm and inviting — not anxious or hype-y?

---

## Step 6: Present All Outputs

Present in this order, with clear section headers:

```
# GAME DAY [#] — FULL CONTENT PACKAGE
## [DATE] | [VENUE]

---
# GAME LISTINGS & POSTER COPY

## [GAME 1 TITLE — SYSTEM]
[poster hook / bullet tags / meetup description]

---
## [GAME 2 TITLE — SYSTEM]
[...]

---
# DISCORD GAME SPOTLIGHTS

## Spotlight — [GAME 1 TITLE]
[Discord post copy]

## Spotlight — [GAME 2 TITLE]
[...]

---
# EVENT-LEVEL POSTS

## Meetup Group Discussion Post
[Copy]

## Venue Discord Reply
[Copy]

---
# NOTES & FLAGS
[Any submission gaps, fallbacks used, items needing Zack's confirmation]
```

Always end with a Notes & Flags section, even if brief. Flag:
- Any field where fallback logic was used
- Any GM submission ambiguity worth confirming before publishing
- Any listing URL that was not provided (spotlight post will have a placeholder)
- Whether floor lead confirmation is still needed

---

## Integration Notes

**game-listing-generator:** Step 2 of this skill runs the full listing
generator logic for each game. All rules from that skill apply — tone
detection, fallback logic, format determination, quality checklist.
Do not shortcut it.

**community-event-promotion:** Step 3 and Step 4 draw on the Discord
spotlight template and event-level post guidance from that skill. All
platform rules, tone matrix, and checklist items from that skill apply.

**steel-city-rpg-promo:** This skill handles game-level and event-level
content generation. The promo orchestrator handles cross-community
promotion (Reddit, Heylo, sister Discords, etc.). Run this skill first,
then hand off to steel-city-rpg-promo for the broader platform push.

**Canva workflow:** Poster hooks and bullet tags from Step 2 paste directly
into the Canva template. No reformatting needed.

**Notion / automation (future):** When the Games DB is live, outputs map
directly to: `meetup_description`, `poster_hook`, `bullet_tags`. Discord
spotlight copy maps to a `spotlight_post` field if added to the DB schema.
