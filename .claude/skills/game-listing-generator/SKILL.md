---
name: "game-listing-generator"
description: "Turns a Steel City RPG GM game submission into a database-ready copy bundle, or links a rerun to its existing copy: listing, title, poster copy, summaries, channel copy, GM DM, alt text."
---

# Game Listing Generator

One submission in, one complete copy bundle out. The bundle is designed to be
entered into the Notion database once, then reused, filtered, and revised
without regenerating from scratch.

Output quality scales with input quality; fallback logic fills gaps so Zack
does not have to chase the GM for every missing field.

## Design principles

1. **Copy is date-agnostic.** A submission may cover several Game Days. Copy is
   written once and reused across dates. Never hardcode "this Sunday" or a
   date into copy. Use the two publish-time tokens below.
2. **Only these tokens exist:** `{{DATE_PHRASE}}` (resolved at publish: "this
   Sunday" only within a 5-day window, otherwise the specific date, e.g. "on
   November 8") and `{{MEETUP_URL}}` (the individual game's full
   `meetup.com/steel-city-rpg/events/[ID]/` URL; never a meetu.ps short link).
   Three more are per-run and filled in when a listing or post is made:
   `{{EVENT_NAME}}` and `{{EVENT_URL}}` (the parent Game Day event for that
   run, used in the closer) and `{{GM_HANDLE}}` (filled from the GM Roster at
   posting time; never store a typed handle in copy). Everything else is
   resolved at generation time.
3. **Separate audiences.** Player-facing copy never contains GM-facing
   content, and vice versa. The GM confirmation DM is the only GM-facing item.
4. **Nothing publishes without GM review.** Every bundle is a draft.
5. **Stable and modifiable.** The source pitch is stored verbatim alongside the
   copy so any piece can be regenerated or edited later (see Revisions).
6. **Never invent.** No fabricated plot details, mechanics, character names,
   scarcity, table counts, or statistics.
7. **Map before you write.** Field-mapping errors poison every artifact. Map
   columns using the known form schema (Step 1), verify the mapping against
   anchors, and show the mapping in the output. Ask before generating if the
   mapping cannot be verified.
8. **One game, one copy row.** A rerun or a "just dupe the last one"
   submission creates new Game Run rows that link to the existing copy. It
   does not get a new bundle (Step 0).
9. **The GM's words carry the game.** Published listings were pulled back
   toward the GM's own wording whenever a draft tightened, joked over, or
   embellished it. Keep distinctive GM lines; add structure, not voice.
10. **Write for the Meetup editor.** Copy is pasted by hand. Formatting that
   does not survive the paste gets published broken (Step 5 rules).

---

## What this produces

For each submission:

| # | Artifact | Audience | Notes |
|---|---|---|---|
| 0 | Submitted Game Days | internal | Normalized venue + date list |
| 1 | Poster hook | player | Exactly 2 lines |
| 2 | Bullet tags | player | 3 tags default, `・` separated |
| 3 | Meetup description | player | 100-200 words |
| 4 | One-line summary | player/internal | 120 characters max |
| 5 | Short summary | player/internal | 40-60 words, neutral |
| 6 | Table card blurb | player | 20 words max, for printed table tents |
| 7 | Discord spotlight | player | Single post, tokens for date and URL |
| 8 | Reddit lineup line | player | One line for r/lfg or community-thread lists |
| 9 | Instagram blurb + hashtags | player | Carousel caption slot |
| 10 | Public thanks line | player | One sentence, reusable across channels |
| 11 | GM confirmation DM | GM only | Private, single-purpose |
| 12 | Poster alt text | player | Accessibility |
| 13 | Database record | internal | JSON: Game Runs rows plus Copy row |

No GM name or handle appears in artifacts 1-6 or 12. No emojis anywhere.
GM handle or first name appears only in artifacts 7-11 as specified below.

---

## Step 0: Check for an existing game first

One game has one copy row. Each date it runs is a separate Games Record row
linked to that copy row.

- If the GM says to reuse or dupe an earlier game, or the title and GM match
  a game that already has copy: do not generate a bundle. Output only the
  Submitted Game Days (Step 1b), one `game_runs` row per date, the listing
  title per run, and a note naming the existing copy row to link.
- If the same GM resubmits the game with a changed pitch, treat it as a
  revision (see Revisions), not a new game.
- If the same title arrives from a different GM, or you cannot tell whether
  it is the same game, ask Zack.

The game-copy-tracker skill does the lookup and the Notion entry.

---

## Step 1: Parse the submission

Submissions usually arrive as a pasted form row, tab-separated, often without
headers. Cells can be quoted and contain line breaks or tabs (the long pitch
field usually does); treat a quoted cell as one field.

### Step 1a: The known form schema (25 columns, in order)

The submission form has these columns. When a pasted row has no headers, map
cells **positionally** to this list. Do not guess columns from content when the
row matches this layout. Blank cells still occupy a position, so count tabs.

| # | Form column | Maps to |
|---|---|---|
| 1 | Timestamp | `SUBMITTED_AT` |
| 2 | Email Address | `GM_EMAIL` (never output) |
| 3 | Preferred Name | `GM` name (first token = first name) |
| 4 | Best Email Address | `GM_EMAIL` alt (never output) |
| 5 | Discord Handle | `GM` handle |
| 6 | What part of Pittsburgh... | `REGION` (database only) |
| 7 | Game System | `SYSTEM` (see Step 1c if "Other") |
| 8 | Game Title | `TITLE` |
| 9 | Game Description | extra `PITCH` material (often blank) |
| 10 | Game Image | `IMAGE` (usually a promise to post one; goes to the DM) |
| 11 | Time slot | `SLOT` (often blank; the Event column governs) |
| 12 | Organizers may contact me | acknowledgement, ignore |
| 13 | Welcoming environment agreement | acknowledgement, ignore |
| 14 | Which Event are you submitting for? | `DATES` (Step 1b) |
| 15 | Meetup Username | `MEETUP_USER` (often an email address; see below) |
| 16 | Run A Game With Us Before? | `GM_NEW` ("No" = first time with Steel City RPG) |
| 17 | Content Warnings? | `CONTENT` (the only source of content notes) |
| 18 | Game Hook | `GM_HOOK` (GM's tagline; input, never published as written) |
| 19 | Pre-Generated Characters Provided? | `CHARGEN` |
| 20 | How Many Players Are You Looking For? | `PLAYERS` (often "Other (tell us in your game description)") |
| 21 | Tell Us About Your Game | `PITCH` (main pitch) |
| 22 | Tone & Vibe | `GENRE` |
| 23 | Anything Players Should Know Going In? | `PLAYER_INFO` |
| 24 | Instagram Handle | `GM_IG` (database only) |
| 25 | Safety Tool Commitment | acknowledgement, ignore |

If headers are present, use them instead and trust them over this table.

**Verify the mapping before writing anything.** Anchors: column 1 parses as a
timestamp, column 2 contains `@`, column 14 contains a date and usually `·`,
column 22 contains genre words (Horror, Comedy, Drama, Action...). If the cell
count is not 25 or an anchor fails, the form has probably changed: stop, show
the mapping you would use, and ask Zack to confirm before generating. Never
silently fall back to guessing.

Print a short **Field mapping check** (see Step 11) with the resolved values for
SYSTEM, TITLE, CONTENT, PLAYER_INFO, GM_NEW, and CHARGEN so errors are visible
immediately.

### Step 1b: Submitted Game Days

Parse `DATES`. Entries are typically comma-separated, each in the form
`Month D · start-end · Venue`. For each entry produce a normalized row:

- **Venue** (as written)
- **Date** in ISO form with weekday, e.g. `2026-11-08 (Sun)`
- **Start and end time**

Year rule: use the next occurrence of that month and day after the submission
timestamp. Verify the weekday; if the date does not land on the venue's usual
Game Day weekday, flag it.

Venue + Date is the stable join key for events. Never use or invent a "Game
Day #"; those are assigned after the fact and may be renumbered.

Each submitted date becomes one Game Run row in the database record (Step 9).
If `DATES` is blank, unparseable, or lists a date in the past, flag it and
still generate the copy. If `SLOT` (column 11) is filled and conflicts with the
Event column, flag it.

### Step 1c: Title, system, and "Other"

`TITLE` and `SYSTEM` are different fields and must never be swapped or merged
by guesswork.

- **TITLE** is whatever the GM typed in Game Title, used as submitted.
- **SYSTEM** is the Game System dropdown. If it names a system, use it.
- If SYSTEM is "Other (tell us in your game description)": look for the system
  name in `PITCH`, `GM_HOOK`, and `PLAYER_INFO`. If none is named, check
  whether the title or pitch matches a well-known published game (a quick web
  search is fine). A confident match is acceptable but must be stated in
  Submission Notes as an inference for Zack to confirm. If there is no
  confident match, treat SYSTEM as missing (Step 2).
- Title and system are often the same (title is the published game name).
  When they differ, the title is the primary identifier and the system
  appears in context (orientation line, Reddit parenthetical, summaries).
- **Title clarity check.** If the title is all caps, joke-like, or would not
  tell a stranger scrolling Meetup what kind of game this is, use it as
  submitted but flag it in Submission Notes, offer one alternative that keeps
  the GM's wording and names the game, and ask about it in the GM DM. Never
  rename a game on your own.

### Step 1d: Field handling notes

| Field | Handling |
|---|---|
| `CONTENT` (col 17) | The only source of the verbatim `Content notes:` line. Blank, "No", or "None" = `None stated`. |
| `PLAYER_INFO` (col 23) | Not a content warning. If present, reproduce verbatim as a `Good to know: [text]` line in the description (same position as the content notes line). Store in `players_know_verbatim`. Never relabel it as a content warning, and never merge it with `CONTENT`. |
| `GM_NEW` (col 16) | "No" means first game with Steel City RPG. Add a warm welcome to the DM. Never mention it in player-facing copy. |
| `GM_HOOK` (col 18) | Treated as GM-provided pitch material, rewritten like any other pitch. Strong phrases may be reused after rewriting. |
| `PLAYERS` (col 20) | If it says "Other", read the max (and min if given) from the pitch. If none is stated, `player_min` / `player_max` stay `null`; do not invent. |
| `IMAGE` (col 10) | If the GM promises to post an image somewhere, acknowledge it in the DM only. |
| `MEETUP_USER` (col 15) | If the value is an email address, it is not a username: do not output or store it, and note that the real username was not captured. |
| `GM_EMAIL`, acknowledgements | Never copied into any output. |
| `REGION`, `GM_IG` | Database only. |

Ignore any cell you cannot place and mention it once in the notes.

---

## Step 2: Fallback logic

Apply per missing field. Never invent specifics.

- **SYSTEM missing (after Step 1c):** Flag immediately and do not generate. Say:
  "No system name in this submission. I need at least the game or system name
  to generate accurate copy; everything else can be inferred."
- **PITCH provided:** Rebuild the pitch in Steel City RPG voice, but keep the
  GM's own distinctive lines, jokes, proper nouns and brand names as written.
  For comedy games, or when the GM's text already reads as finished listing
  copy, stay close to it: fix the structure and add the required lines, do
  not rewrite the voice. Extract scenario,
  stakes, named factions and locations, character framing, specific mechanics,
  and any genuinely strong phrases, then rebuild in Steel City RPG voice using
  Step 5. Improve passive or flat language, hedging, jargon that loses
  newcomers, and anything that undersells the table.
- **Strip entirely from player-facing copy:** first-person GM statements ("I'll
  be there early"), GM self-assessments ("first time running this"), table
  logistics, "message the GM" language, and image-upload promises (note them
  for the GM DM instead).
- **PITCH missing or one line long:** Write a light premise so the listing is
  usable: one or two sentences built from the title, the Game Hook, the
  system and the genre boxes. Keep it general. Do not name characters,
  places, factions, groups or plot turns the GM did not give. Say in
  Submission Notes which sentences are invented, and put two or three
  follow-up questions in the GM DM (what the players are doing, who they
  are, what is at stake) so the premise can be replaced with the GM's own.
  Generate the rest from `SYSTEM` + `GENRE` using the system's design
  identity (narrative: lead with story and stakes; crunchy: lead with
  decisions; GM-less: lead with the shared experience). Unknown system, use:
  "A complete adventure in a single session. No prep, no commitment, just you
  and the table finding out what happens."
- **GENRE missing:** Default to the system's known aesthetic; otherwise neutral
  adventure language. Never default to fantasy or imply D&D.
- **EXP missing:** Beginner-friendly, and "No experience needed" stays in the
  copy, including when characters are built at the table. Leave it out only
  when the GM says experienced players are wanted. If a level above 1 is
  stated or the system is crunchy (D&D, Pathfinder), keep the line, flag it,
  and ask the GM in the DM to confirm it fits. If the
  system is notably harder for
  newcomers, flag that "no experience needed" depends on pre-gens and teaching
  at the table.
- **Pre-gens are always the default.** Use "Pre-generated characters
  provided" unless the GM says otherwise.
- **CHARGEN says "Partially":** Pre-gens that are finished at the table.
  Write "Pre-generated characters provided, finished at the table." Do not
  write "mostly built" or drop the pre-gens. Ask the GM in the DM what is
  left to choose.
- **CHARGEN says "No - created as part of the game":** Characters are made at
  the table; no pre-gens language. "No experience needed" still applies.
- Every artifact must agree with CHARGEN, including the spotlight, tags and
  table card.
- **CHARGEN missing:** Pre-generated characters provided. If CHARGEN is
  provided (for example "Yes - Fully Prepped"), it is confirmed; say so in
  the record.
- **CONTENT provided:** Reproduce verbatim, never softened or condensed. Place
  as a line reading `Content notes: [verbatim text]` just before the
  accessibility line, and store verbatim in the database. If the field says
  "No", "None", or is blank, store `None stated`, add nothing labeled as
  content notes, and never write that the game has no content concerns.
  Verbatim means the GM's exact words after the label: do not turn the note
  into a bullet, add or drop a word, add punctuation, or swap the label. If
  the GM answered "Yes" with no text, flag it and ask in the DM; the listing
  should not publish without it. If `PLAYER_INFO` names content themes (gore,
  violence, death and similar), keep it under `Good to know:` and flag it for
  Zack.
- **PLAYER_INFO provided:** See Step 1d; verbatim `Good to know:` line in the
  same position. If both CONTENT and PLAYER_INFO exist, CONTENT comes first,
  then Good to know.
- **DATES missing:** Flag; generate copy anyway.

---

## Step 3: Determine FORMAT (GM-led vs. GM-less)

Do not rely on the form. FORMAT comes from the system. Only the systems below
are GM-less or fully collaborative; everything else is GM-led.

**Known GM-less / no-facilitator systems:** Descended from the Queen (and all
For the Queen engine hacks), For the Queen, For the Dragon, Wanderhome,
Ironsworn / Starforged (solo or co-op capable), The Quiet Year, Follow, Fiasco
(facilitator-optional), Dialect, Microscope, Kingdom, Dream Askew / Dream
Apart, Alone Among the Stars, Cthulhu Dark (some modes; confirm if uncertain).

Unfamiliar and not on the list: GM-led; flag only if it would change the copy.

GM-less games never call anyone the GM, in any artifact (use the Fiasco rule:
characters built at the table start, never "pre-gens" language). The person
who brings the game is still thanked by name: the spotlight and Instagram
blurb keep the thanks line, worded as bringing the game, never as running or
GMing it.

---

## Step 4: Tone detection

Classify from the genre checkboxes. Identify the dominant register and use
the secondary as a modifier.

| Checkbox | Register | Copy characteristics |
|---|---|---|
| Horror | Dread / Atmospheric | Slow build, unease, short declaratives |
| Comedy | Lighthearted / Fun | Warm, playful rhythm, question hooks work |
| Drama | Intimate / Narrative | Character-forward, emotional stakes |
| Action / Combat-Heavy | Kinetic / Tactical | Fast, verb-led, consequence language |
| Mystery | Tense / Investigative | Questions over answers, clue language |
| Political Intrigue | Slow Burn / Strategic | Competing interests, loyalty, trust |
| Dungeon Crawl | Classic Adventure | Resource stakes, danger-forward, concrete nouns |
| Social / Roleplay-Heavy | Character-Driven | Relationship language, NPC texture |
| Experimental | Strange / Liminal | Surreal, structural framing OK |
| Other (free text) | Match the text | Read and match |

Multi-tag blending: Horror + Comedy leads with Dread, allows dark humor.
Action + Drama leads Kinetic, grounded in character stakes. If the checked
genre clashes with what the pitch actually describes (for example Action
checked but the pitch is mystery-led), follow the pitch for the body copy and
note the tension in Submission Notes.

System overrides tone when they clash. GM-less games add a collaborative layer
regardless of genre.

---

## Step 5: Write the Meetup description (artifact 3)

### Required structure, in order

1. **HOOK [required]:** 1-3 short lines or a strong question. Genre-true. Do
   not start with the system name or "In this one-shot...". Comedy and light
   games: question hooks work. Horror, action, drama: declaratives or a scene
   in motion.
2. **ORIENTATION [required]:** 1-2 sentences naming the title (bold, first
   mention) and, when it differs from the title, the system, plus format and
   the kind of experience. Include character level naturally if relevant.
3. **PREMISE [required]:** 1-2 sentences: what is happening, who the players
   are in it, what is at stake. Named factions and locations belong here.
4. **PARTY FRAME [conditional]:** 1 sentence. Include only when specific and
   interesting.
5. **BULLET BLOCK or INLINE TAG LINE [required, choose one]:** GM-led uses
   3-5 `*` bullets of short phrases. GM-less uses one comma-separated line
   ("GM-less, collaborative storytelling, no prior experience needed."). Never
   both.
6. **CLOSING LINES [required]:** 1-3 short punchy lines returning to tone or
   raising stakes.
7. **CONTENT NOTES [conditional]:** Only if `CONTENT` was provided; verbatim,
   labeled `Content notes:`.
7b. **GOOD TO KNOW [conditional]:** Only if `PLAYER_INFO` was provided;
   verbatim, labeled `Good to know:`. Sits after Content notes if both exist.
8. **ACCESSIBILITY LINE [required]:** Unless the GM states experienced players
   only. Surfaces pre-gens by default and matches the game's register.
8b. **SIGN-UP AND SERIES LINES [conditional]:** Pathfinder Society games get
   `Please sign up on the Pittsburgh Pathfinder Society Warhorn to confirm your seat.`
   (Zack adds the link). Multi-session games get one line naming the other
   part, linked per listing. A GM's bring-your-own list goes here verbatim.
9. **NOT-YOUR-VIBE CLOSER [required, always last]:**
   `[Contextual lead-in] Check out other events happening for [{{EVENT_NAME}}]({{EVENT_URL}}).`
   The event name and link differ per run, so they stay as tokens in stored
   copy and are filled in for each listing. This line was hand-edited on
   nearly every published listing; check that the lead-in still makes sense
   once the event name is in.
   The lead-in is game-specific (genre redirect, premise callback, or "Not your
   vibe?" only if nothing fits). Always "events", never "games". Ask what the
   most natural reason is for someone to pass on this table and speak to that.

### Rules

- 100-200 words. A tight 100 beats a loose 200.
- Player count and session length are Meetup event fields; leave them out.
- Bold (`**text**`) the title on first mention (the system is plain text when
  it differs). No headers.
- If a TITLE is provided, use it as the primary identifier and reference the
  system for context.
- No scarcity, no table counts, no urgency.
- No em dashes and no "X, not Y" contrast phrasing; both get removed by hand.
- Paste-safe formatting: bullets start with `- `, one per line, with a blank
  line before and after the block. Write the hook and the closing lines as
  single lines or separate paragraphs, never as two lines split by a single
  line break, because Meetup joins them. Bold only the title.
- Nothing about table size anywhere in the body, including bullets, the
  orientation line and closers.
- Do not add setting, lore, group names or framing the GM did not write. The
  one exception is the light, general premise allowed when the pitch is
  missing (Step 2), which is flagged as invented. For
  homebrew settings, keep the GM's detail instead of compressing it away.

### Listing title (artifact 3a, one per run)

`[System] / [Title] / [Area] RPG Day`, for example
`Monsterhearts 2 / All This and Some Pretzel Nuggets / Monroeville RPG Day`.
Areas: Brookline (Halfling's Hideout), Lawrenceville (Lawrence Hall),
Monroeville (Guy on the Couch), Robinson (Four Horsemen). When title and
system are the same, use it once. Multi-session games add `- Part N` after
the title. No Game Day number. The title in the headline and the title in
the body must match.

---

## Step 6: Write the poster hook (artifact 1)

**Exactly 2 lines. No exceptions.**

Canva spec: Immortal, size 30, 950px max width. Keep each line under about 40
characters so it sits on one row.

- An invitation, not a summary. Fragments preferred; rhythm over grammar.
- Line 1 sets the stage; line 2 sets the stakes or tension.
- Match the genre register. Do not name the system or title. No accessibility
  language.
- Draw from strong GM phrases but always rewrite.

Register examples: Dread: "Something followed you in from the dark. / The
others are starting to act strange." Comedy: "The heist was supposed to be
simple. / It has not been simple for some time." Kinetic: "Neon. Concrete.
Corporate control. / How far do you go to get paid?" Mystery: "Three suspects.
One locked room. No good answers. / Someone in this house is lying."

---

## Step 7: Write the bullet tags (artifact 2)

Separator is `・` (U+30FB), which renders star-like in Immortal; that is
correct. Canva: Immortal, size 25. Three tags by default; four only if each is
3 words or fewer and the whole line stays under about 50 characters, counting
spaces and separators.

- **Slot 1, accessibility/format (always):** No prep needed, No experience
  required, Beginner friendly, Pre-gens provided, One-shot format, Drop-in
  friendly
- **Slot 2, experience type:** High-stakes decisions, Collaborative story
  game, Cinematic action, Tactical gameplay, Narrative-first,
  Improvisation-forward, Shared world-building, Fast character creation,
  Diceless gameplay, Resource management
- **Slot 3, GM structure/mechanics:** GM-less, Collaborative storytelling,
  Player-driven, Jenga mechanic, OSR gameplay, Prompt-driven, Facilitator-led
- **Slot 4, genre/mood (optional):** Corporate dystopia, Street-level
  survival, Survival horror, Political intrigue, Cozy adventure, High fantasy,
  Cosmic horror, Noir mystery, Grimdark, Heist, Sci-fi thriller

If slot 3 has no honest mechanics tag, use a short genre word instead
("Intrigue", "Mystery") or an honest GM-stated descriptor ("Rules-light")
rather than inventing a mechanic.

---

## Step 8: Companion copy (artifacts 4-12)

Write these after the core three so tone stays consistent. All are rewrites,
never reuse a sentence from the Meetup description verbatim.

### 4. One-line summary
120 characters max. Plain, no hype, no jargon. Formula: who you are + what
happens. Used in dropdowns, database views, email lists, recap tables.

### 5. Short summary
40-60 words, third person, neutral and factual. Names the title (and system
when different), the scenario, and the kind of play. No marketing flourish and
no GM name. This is the reuse workhorse: recaps, newsletters, Notion cards,
sponsor or venue summaries.

### 6. Table card blurb
20 words maximum, for printed table tents (ink-economy print system). Title is
shown separately, so write the pitch fragment plus one accessibility fragment
("Pre-gens provided." or "Characters made at the table.").

### 7. Discord spotlight
One post, casual, no emojis, no hashtags. Template:

```
[Hook line, adapted from the poster hook, never pasted flat]

**{{GM_HANDLE}}** is running **[Game title]** {{DATE_PHRASE}} - [tight premise
sentence]. [One key sell sentence: system angle, tone, or mechanic; omit if
nothing distinct.]
[Public thanks line]

[Accessibility line] Grab a seat: {{MEETUP_URL}}
```

Accessibility line, exact phrasing: "No experience needed, pre-gens
provided." If the game has no pre-gens (characters made at the table, GM-less,
Fiasco-style), use "No experience needed, characters are made at the table."
If CHARGEN is "Partially", use "No experience needed, pre-gens provided and
finished at the table."
GM-less games open with the title instead of `**{{GM_HANDLE}}** is running`,
and keep the thanks line ("for bringing this one").

### 8. Reddit lineup line
One line for reuse in r/lfg lineup lists or community threads:
`**[Game title]** ([System]) - [one sentence pitch, 25 words max]. [Meetup]({{MEETUP_URL}})`
No GM name, no scarcity, no hashtags, and never an online tag (every game is
in person). When title and system are identical,
write the parenthetical as the system or publisher context (for example
"Havoc Brigade RPG"). This is a building block, not a post: Reddit posts are
assembled at event level and held until every individual Meetup URL is
confirmed live.

### 9. Instagram blurb
1-2 sentences, 30 words maximum, warm and photo-forward. Use the GM's first
name (not the handle). End with a link-in-bio cue only if the post type needs
one. Then a separate hashtag block: `#SteelCityRPG #PittsburghEvents
#TabletopRPG #GameNight #TTRPG` plus up to two system or genre tags.

### 10. Public thanks line
One sentence, reusable on any channel; use `[GM ref]` as handle on Discord and
first name elsewhere (write it with the handle and note the swap). Draw from
this library and avoid repeating a phrase within the same event (final
rotation is applied at event assembly):
- Big thanks to [name] for bringing this one.
- Thanks to [name] for making this one happen.
- Grateful to have [name] at the table for this one.
- [Name] is the reason this one exists. Thank you.
- Huge shoutout to [name] for running this.
- Thank you to [name] for putting this together.
- Couldn't do this without [name]. Thanks for running it.

### 11. GM confirmation DM (GM only)
Private, warm, low-pressure, single-purpose, about 100 words:
- Thank them for submitting; name the game. If `GM_NEW` is "No", add a short
  welcome to their first Steel City RPG game.
- Confirm exactly which dates and venue they signed up for.
- Show the Meetup description for review and ask them to confirm or send
  edits.
- If `CONTENT` or `PLAYER_INFO` was given, confirm it will appear exactly as
  written, under its own label.
- If the title was flagged by the clarity check, ask about it here.
- Mention any other open question (character creation, image upload, dates).
- Say they will get the Meetup link once it is live.
- No obligation language, no pressure, no scarcity.

### 12. Poster alt text
Under 125 characters: poster for the game title and the two hook lines in
plain prose. No emojis.

---

## Step 9: Database record (artifact 13)

Emit one JSON block with two tables, following the existing Notion design:
copy lives in its own table, related to Game Runs; the Meetup link lives on
the Game Run only.

**`game_runs`: one row per submitted date**

```json
{
  "game_name": "",
  "system": "",
  "venue": "",
  "date": "YYYY-MM-DD",
  "start_time": "",
  "end_time": "",
  "status": "Drafted",
  "listing_title": "",
  "submitting_gm_handle": "",
  "running_gm_handle": "",
  "gm_first_name": "",
  "gm_new_to_scrpg": false,
  "gm_instagram": null,
  "region": "",
  "meetup_username": null,
  "player_min": null,
  "player_max": null,
  "meetup_url": null,
  "submitted_at": "YYYY-MM-DDTHH:MM"
}
```

`running_gm_handle` defaults to the submitting GM. Status uses the Games
Record values (Drafted, Published, Announced, Consolidated, Ran, Cancelled)
and always starts as `Drafted`; older notes may call this
`Submitted`. `meetup_username` stays `null` if the form value was an email.

**`game_copy`: one row per game** (reruns link to the same row; see Step 0)

```json
{
  "game_name": "",
  "copy_version": 1,
  "copy_status": "Draft",
  "copy_source": "Chat-generated",
  "generated_at": "YYYY-MM-DD",
  "format": "GM-led",
  "tone_register": "",
  "genre_tags": [],
  "experience": "",
  "chargen": "",
  "level": null,
  "content_notes_verbatim": "None stated",
  "players_know_verbatim": "None stated",
  "poster_hook": "",
  "bullet_tags": "",
  "meetup_description": "",
  "one_liner": "",
  "summary_short": "",
  "table_card": "",
  "discord_spotlight": "",
  "reddit_line": "",
  "instagram_blurb": "",
  "hashtags": "",
  "thanks_line": "",
  "gm_dm": "",
  "alt_text": "",
  "source_pitch": "",
  "flags": []
}
```

`copy_status` moves Draft, GM Approved, Published. `source_pitch` holds the
GM's original text verbatim (the Game Hook plus Tell Us About Your Game, and
Game Description if filled; internal only, never published) so any piece can
be regenerated later. Tokens `{{DATE_PHRASE}}` and `{{MEETUP_URL}}` stay
unresolved inside stored copy. Use real line breaks as `\n`, and escape double
quotes. Do not include the GM email.

---

## Step 10: Quality check

- [ ] Columns mapped by the known schema (or headers), anchors verified, and
      the Field mapping check printed?
- [ ] TITLE and SYSTEM kept separate? "Other" system resolved from the
      description or flagged as an inference?
- [ ] CONTENT (col 17) and PLAYER_INFO (col 23) kept separate, each under its
      own label, never swapped?
- [ ] GM_NEW used only in the DM?
- [ ] Title clarity check run (all caps, joke title, unclear genre)?
- [ ] Tone register consistent across every artifact?
- [ ] No invented specifics, scarcity, table counts, or statistics?
- [ ] GM pitch rewritten, not passed through? Operational notes stripped?
- [ ] Description follows the required order, with closing lines, accessibility
      line, and the "events" closer with a game-specific lead-in?
- [ ] Content notes verbatim, or `None stated` with nothing labeled as content
      notes added to the copy?
- [ ] Poster hook exactly 2 lines, each about 40 characters or fewer, no
      system or title?
- [ ] Tags: `・` separator, 3 words or fewer each, line under about 50
      characters?
- [ ] FORMAT taken from the GM-less list, not assumed? GM-less games call no one the GM but still thank the person bringing it?
- [ ] No GM name or handle in artifacts 1-6 and 12? No GM email (or
      email-as-username) anywhere?
- [ ] No hardcoded dates; only `{{DATE_PHRASE}}` and `{{MEETUP_URL}}` tokens?
- [ ] Submitted dates normalized, weekday checked, one Game Run row per date,
      no "Game Day #"?
- [ ] Summary lengths within limits (120 characters, 40-60 words, 20 words)?
- [ ] Nothing implies D&D as default? Beginner-friendly unless the GM said
      otherwise?
- [ ] Zero emojis anywhere?
- [ ] Step 0 run: is this a rerun or dupe that should link to existing copy?
- [ ] Closer uses `{{EVENT_NAME}}` and `{{EVENT_URL}}`; listing title written
      for every run and matching the body?
- [ ] Content notes and Good to know are the GM's exact words, unbulleted?
- [ ] Every artifact agrees with CHARGEN (pre-gens by default; "Partially"
      means finished at the table)? "No experience needed" kept unless the GM
      asked for experienced players?
- [ ] If the pitch was missing, is the invented premise general, flagged in
      Submission Notes, and followed up in the GM DM?
- [ ] No em dashes, no table size in the body, `- ` bullets, no hook or
      closer split by a single line break?
- [ ] Every proper noun, setting detail and group name traceable to the GM's
      text?

---

## Step 11: Present and flag

Per game, in this order:

```
# [TITLE] - [SYSTEM]
**Submitted for:** [Weekday, Month D, YYYY] and [...] | [Venue] | [times]
**Format:** GM-led/GM-less | **Register:** [dominant + modifier]

**Field mapping check:** System: [x] | Title: [x] | Content warnings: [x or None stated] | Players should know: [x or none] | First time with SCRPG: [yes/no] | Pre-gens: [x]

## Publish copy
**POSTER HOOK** ... **BULLET TAGS** ... **LISTING TITLE (per run)** ... **MEETUP DESCRIPTION** ...

## Short-form
One-liner / Short summary / Table card

## Channel copy
Discord spotlight / Reddit lineup line / Instagram blurb + hashtags

## Thanks and GM-facing
Public thanks line / GM confirmation DM

## Accessibility
Alt text

## Submission notes

## Database record
[JSON]
```

For batches, separate games with `---`.

**Stop after presenting.** The bundle is a proposal. Do not write the copy,
the Game Run rows or anything else to Notion in the same turn, even when the
Notion tools are available. End by asking Zack whether the copy is okay to
enter. Only his explicit okay hands the bundle to game-copy-tracker; if he
asks for changes, revise, present again and wait.

**Submission notes:** list only what matters: fields where a fallback
meaningfully changed the output, system inferences (Step 1c), title clarity
flags, date anomalies, conflicts between checked genre and pitch, and anything
to confirm with the GM before publishing. Do not flag every missing optional
field. End with one or two concrete questions for Zack when something blocks
publishing.

**Ask before generating** when: the cell count or anchors do not match the
known schema, SYSTEM cannot be resolved, or two fields could plausibly be
swapped in a way that changes the copy. A one-line question costs less than a
full regenerated bundle.

---

## Revisions

When asked to change one piece ("make the Discord post punchier", "redo the
hook", "the GM changed the pitch"), or when Zack corrects the field mapping:

1. Regenerate only the requested artifact, keeping the same tone register.
2. If the pitch, title, system, or content fields changed, regenerate
   dependents (description, summaries, spotlight, Reddit line, Instagram
   blurb, alt text, DM) and say so. Artifacts that do not name the changed
   field stay as they are; say which.
3. Return only the changed fields as a JSON patch with `copy_version`
   incremented and `copy_status` reset to Draft.
4. Never silently alter approved copy; show what changed.

---

## Reference files

`references/examples.md` has annotated real listings (Dread, Rise of the
Redscales, Ringmaster, The Expanse RPG, Edirne, Shadowdark). Read it when
calibrating tone, structure, or closers, or for an unfamiliar genre.

## Integration notes

- **game-day-content-generator:** Calls this skill per game, then assembles
  event-level posts (Meetup group post, venue reply, resolved spotlights with
  real dates and URLs, thanks rotation). Artifacts 7-10 are building blocks it
  resolves.
- **community-event-promotion / steel-city-rpg-promo:** Cross-platform pushes
  consume the Reddit line, Instagram blurb, and summaries from the stored
  record instead of rewriting from scratch.
- **Canva:** Hook and tags paste directly into the template (hook in the large
  text block, tags in the `・` row).
- **Notion:** entry, run linking and published-copy capture are handled by
  the game-copy-tracker skill. `game_runs` and `game_copy` map to the Games Record and Game Copy tables;
  field names above are the intended column names. New columns this version
  adds: `gm_new_to_scrpg`, `gm_instagram`, `region`, `meetup_username` on Game
  Runs; `players_know_verbatim` on Copy.