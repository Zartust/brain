---
name: community-event-promotion
description: >-
  Cross-platform event promotion copywriting for Steel City RPG. Use this skill
  whenever Zack needs to promote an event across multiple platforms or channels
  including Discord servers, Reddit subreddits, Heylo, Meetup, or any community
  space. Triggers include: write posts for my event, I need to promote Game Day,
  write something for a specific platform or server, help me post about this, or
  any request involving promoting an event to an external community. Also
  triggers when Zack shares a previous post as a reference example, asks about
  whether to include images or embeds, or needs platform-specific formatting
  advice. Adapts tone, length, and format based on community warmth, platform
  type, and standing in that community.
---

# Community Event Promotion

A skill for writing cross-platform event promotion copy for Steel City RPG Game Days and other events.

## Step 1: Gather Event Details

Before writing anything, always fetch the live Meetup event page if a link is provided. Do not rely on memory or previous conversation — details like confirmed games, times, and descriptions change.

```
web_fetch: [meetup event URL]
```

Extract: event name, date, time, venue name, venue address, neighborhood, confirmed games/systems, Discord link, any special details.

---

## Step 2: Understand the Destination

Ask or infer the following before writing. These determine everything about tone and length:

### Platform Type
| Platform | Default Style |
|---|---|
| Active Discord (self-promo channel) | Punchy, community-framed, medium length |
| Active Discord (events/meetings board) | Scannable, structured, bullet-friendly |
| Active Discord (sister/partner community) | Casual, friend-to-friend, short |
| Active Discord (LGS — DnD General etc.) | Hook-led, pain-point aware, GM pitch included |
| Dead Discord server | Low-key, personal, brief — like leaving a note |
| Heylo (social meetup group) | Match previous post length/tone; usually brief |
| Reddit (own post) | Longer, structured, community-appropriate framing |
| Reddit (weekly thread comment) | Short and scannable — competing with other replies |
| r/lfg specifically | Must include system tags in title; longer body OK |

### Community Warmth
| Standing | Tone |
|---|---|
| Active member, they know you | Assume familiarity, skip the intro |
| Have permission, not very active | Acknowledge you were given permission; don't overclaim familiarity |
| Sister/partner community | Talk like you're telling friends about your thing |
| Cold outreach / no history | Lead with value, be transparent about what you are |

### Always ask if not obvious:
- Is there a previous post from this venue/platform to match voice to?
- Is it a standalone post or a thread comment?
- Should it include a GM recruitment angle?

---

## Step 3: Platform-Specific Rules

### Discord: Image vs. Link
- **Link only** → Discord auto-embeds the Meetup page (title, date, image). This is cleaner for active servers. Recommended default.
- **Link + PNG flyer** → Redundant in active servers; feels like an ad blast. Avoid unless the server has no embed previews or is a dead server.
- **PNG only** → Never for Discord — no clickable RSVP path.

### Reddit: Research Rules First
Before writing any Reddit post, check subreddit rules. Key rules encountered:
- **r/lfg**: Requires system tags in title format `[Offline][City, State][System]`. Can combine e.g. `[D&D 5e/Various Systems]`. Weekly thread posts can be longer than typical comments.
- **r/pittsburgh**: Events belong in the weekly community thread, not standalone posts.
- **r/PittsburghSocialClub**: No general event posts — requires mod permission exception. Message mods first (see mod message template below).
- **r/pittsburghevents**: Verify this subreddit exists and is active before investing time.

### r/lfg System Tag Strategy
If multiple systems are running:
- Use confirmed systems in the tag: `[D&D 5e/Various Systems]`
- Name confirmed systems in the body
- Add `[Recurring]` tag if the event repeats — attracts community-seekers not just one-off players
- Per-game individual posts are high-value but high-effort — batch these when automation is set up, not day-of

---

## Step 4: Tone & Voice Patterns

### Pain-point hooks (use for LGS / experienced player audiences)
> "Already knee-deep in a campaign and can't commit to another one? That's exactly what Game Days are for."

### Beginner-friendly framing (use for general social audiences)
> "If you've ever been curious about tabletop RPGs but didn't know where to start — this is genuinely a great entry point."

### Community/social framing (use for Heylo, r/PittsburghSocialClub)
> "It's honestly as much a social event as it is a gaming one."

### Friend-to-friend (use for sister communities / warm Discord servers)
> "If you've been meaning to come out or hankering to try more RPGs, give it a gander 🎲"

### Events board (scannable listing format)
Lead with bolded title including date, follow with one punchy sentence, then bullets, then RSVP link.

---

## Step 5: Post Length Guide

| Context | Target Length |
|---|---|
| Dead server | 3–4 sentences + link |
| Heylo (match previous) | Match prior post — usually 2–3 sentences |
| Sister Discord | 3–5 sentences, casual |
| Self-promo channel | 1 short paragraph + bullets or details + link |
| Events board | Title + 1 sentence + bullets + link |
| LGS DnD channel | Hook opener + structured bullets + GM pitch + link |
| Reddit standalone post | Full structured post with sections |
| Reddit thread comment | 2–3 sentences max + link |
| r/lfg post | Tagged title + event overview + how-it-works + bullets + links |

---

## Step 6: Reusable Templates

### Mod Permission Message (e.g. r/PittsburghSocialClub)
When a subreddit requires mod approval for event posts but states they're open to exceptions:
- Open by referencing their stated exception criteria
- Describe the event in terms of *their* community values (free, designed to meet people)
- Frame the RPG angle as secondary to the social angle
- Keep it short — mods are volunteers
- End with genuine opt-out ("totally understand if it's not the right fit")

### GM Pitch Line (add to LGS/experienced player posts)
> "We've also got open GM slots if you've been looking for an excuse to run something. One-shot, system demo, weird experiment — we'll take it."

### Recurring Event Signal (add when event repeats)
> "If you miss this one, there's always a next one."

---

## Step 7: Always Check

Before finalizing any post:
- [ ] Is the event number correct? (Game Day 2 → 3 → 4 etc.)
- [ ] Is the date accurate and unambiguous? ("this Sunday" vs "Sunday the 29th" — don't use "this" if the event is more than ~5 days away)
- [ ] Is the session length correct? (currently 4 hours, 4–8 PM)
- [ ] Is the Meetup link the live event link, not a previous event?
- [ ] Is the Discord invite link current? (https://discord.gg/7E46MeGQCs)
- [ ] Does the GM recruitment angle belong in this post?
- [ ] Does the image/embed decision match the platform?

---

## Reference: Known Platforms & Communities

| Community | Type | Warmth | Notes |
|---|---|---|---|
| The Bugbear | Discord, sister community | Warm — active member, shared Meetup | Casual friend tone; shared membership with organizer |
| LGS Discord (DnD General) | Discord, active | Permitted, engaged | Hook with scheduling pain; include GM pitch |
| LGS Discord (meetings/events board) | Discord, active | Permitted | Scannable listing format |
| Dead Discord | Discord, inactive | Only poster | Low-key personal note; PNG + link both OK |
| Heylo Pittsburgh social group | Heylo, active | Permitted, not very active | Match previous post brevity |
| r/pittsburgh | Reddit, very active | Post in weekly thread | Short thread comment |
| r/PittsburghSocialClub | Reddit, active | Requires mod permission | Message mods first; social framing over RPG framing |
| r/lfg | Reddit, active | Open | Required system tags; weekly thread allows longer posts |
| r/pittsburghevents | Reddit, unknown | Unverified | Confirm subreddit exists before posting |

---

## Integration with Other Skills

- **brand-voice-builder**: If voice feels off, check brand voice guidelines before writing
- **content-extraction**: If pulling multiple post types from one event, use content-extraction to batch
- This skill focuses on *outbound community promotion* — for Meetup event listing copy specifically, refer to conversation history and established Meetup formatting conventions (markdown, placeholder link syntax, Rations throughline)
