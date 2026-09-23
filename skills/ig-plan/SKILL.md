---
name: ig-plan
description: >-
  Build the week on Instagram - what to post, which format, when, and who to
  engage with. Use when the user says "plan my week", "what should I post",
  "content calendar", "I have nothing to post about", or wants a posting
  schedule and an engagement list.
---

# ig-plan

The control room. Everything else in this pack executes; this decides what gets
executed. Run it once a week, on the same day.

## Input

If the user names a page for this plan, look first for
`~/.claude/instagram/voice-<page>.md` (page name lowercased, spaces and
punctuation turned to dashes) instead of the default `voice.md`. If
`swipe.md` and `log.md` are also page-specific, look for
`swipe-<page>.md` / `log-<page>.md` first and fall back to the shared files.

If the voice file, `swipe.md` and `log.md` exist, read them. The swipe file
is the user's own evidence from `/ig-viral` about which formulas are landing
in their niche right now, and it outranks anything in this file. The log
stops the plan repeating a theme from the last fortnight.

If they do not exist, ask for four things and write them down:

1. What the user sells, and to whom.
2. The three or four themes they want to be known for.
3. What actually happened this week: a client call, a number, a mistake, a
   thing they built, an argument they had. This is where posts come from.
4. Ten accounts worth being visible to.

## What to post

Four to five posts a week, and at least three of them Reels. Reels are the only
format on Instagram that reliably reaches people who do not follow the account.
Carousels go deep with the people who already do. Stories are daily and are
planned separately.

Mix across the week, never two of the same type back to back:

| type | share | job |
| --- | --- | --- |
| **Proof** | 1 per week | something that happened, with a number. Reel. |
| **Teach** | 1 to 2 per week | one thing the viewer can do today. Reel or carousel. |
| **Opinion** | 1 per week | a position that could lose you followers. Reel. |
| **Story** | 1 per fortnight | a scene with a cost. Reel. |
| **Offer** | 1 per fortnight | what you sell, said plainly, no apology. Carousel or stories. |

For each slot give: the theme, the specific angle from what actually happened
this week, the format, and the hook formula number from `ig-reel/hooks.json`.
Not a topic, an angle. "AI" is not a plan. "The proposal we lost because the
draft had an em dash in it" is a Reel.

## When to post

Post when the user's audience is awake and not at work. For most consumer
audiences that is early evening local time; for a business audience, early
morning.

But say this plainly: **the hour matters far less than the first two seconds.**
Instagram will keep showing a Reel for days if it performs, and will bury a
well-timed one that does not. If the user is optimising posting times before
their hooks work, they are polishing the wrong thing, and you should say so.

Anchor times to the audience's timezone, not the user's, if those differ.

## The engagement round, which is not optional

20 minutes a day, before posting, not after. Build a list of 10:

- **5 reach** - accounts with an audience the user wants, where a good comment
  gets seen. Comment early, before the thread is 200 deep.
- **3 peers** - same size, same field. This is the group that reciprocates.
- **2 buyers** - people who could actually buy. Comment for weeks before any
  DM, and never pitch in a comment.

Hand the list to `/ig-comment`.

## Output

```
WEEK OF SEP 15

MON  engage only  (20 min, list below)
TUE  7:30pm  REEL      PROOF    #5  Time Collapse   - 5hr proposal to 20 min
WED  stories only + engage
THU  7:00pm  CAROUSEL  TEACH    Job B caption       - the 4-slide clause breakdown
FRI  7:30pm  REEL      OPINION  #2  Negative Command - stop doing discovery calls
SAT  -
SUN  6:00pm  REEL      STORY    #21 Mid-Sentence    - the refund email

STORIES  every day, 3 to 5 frames, question box on Thursday.

ENGAGE  (5 reach / 3 peers / 2 buyers)
  ...

Say "write Tuesday" and I will draft it.
```

Write the plan to `~/.claude/instagram/plan-<page>.md` if a page was named,
else `plan.md`, so the other skills can read it. Nothing is scheduled or
posted anywhere. This is a plan and the user runs it.

## Long-range / quarterly planning

Everything above is one week. A quarter is a sequence of those weeks, built
once as a skeleton (theme and content quota per week, not full scripts -
scripting 12+ weeks in one pass produces guesses, not plans), then filled in
week by week as `/ig-reel` and `/ig-carousel` actually run.

**Lay the quarter out week by week, Monday-Sunday, real calendar dates -
never assume a holiday's weekday or a month's week count. Compute it.**

**Holiday weeks vs. season weeks.** Where a niche has recurring calendar
holidays (a food/drink/gift page has one most months), give each holiday
two weeks: the week before it and the week containing it. Every other week
in that stretch is a season week - weather, general seasonal relevance, not
tied to the specific date. Do not force an exact split if two holidays land
close together in one month (Christmas and New Year's, 6 days apart, is the
common case): let the back half of one holiday's week double as the next
one's lead-in rather than manufacturing a second full lead-up week, and say
that is what you did.

**Reposting what works.** Never invent which posts are "top" - that comes
from two real sources, both explicit in the plan whenever a repost slot is
still open: (1) the account's own Insights, read via `/ig-audit` once real
numbers exist, and (2) a `/ig-viral`-style scan of what's trending in the
niche right now. A repost is never a re-run of the same asset: same recipe
or idea, different glass/prop/environment, different visual treatment - a
new production of a proven concept, not a duplicate file.

**A non-reel week anchor.** If the account runs a recurring non-reel format
(a "swap this for that" carousel is the common one for a product/recipe
page), derive its content from that same week's reel lineup rather than
writing it independently, unless the user says otherwise - the carousel's
job is to reinforce the week it sits inside, not run a parallel theme.

**Output shape**, one row per week:

```
Q4 2026 - <your page>

W1  Sep28-Oct4   SEASON            4 reels + 1 swap carousel + 2 info carousels
W2  Oct5-Oct11   SEASON            ...
W3  Oct12-Oct18  SEASON            ...
W4  Oct19-Oct25  HOLIDAY lead-up   Halloween (Oct 31, Sat)
W5  Oct26-Nov1   HOLIDAY week      Halloween
...

Say "build week 4" and I will run that week through /ig-reel + /ig-carousel
the way a normal week does. Nothing here is scripted yet - this is the
skeleton the weeks get built against.
```

Write it to `~/.claude/instagram/plan-<page>-q<N>-<year>.md`. The standing
weekly `plan-<page>.md` stays the single source of truth for *this* week;
the quarterly file is the map that week gets pulled from.
