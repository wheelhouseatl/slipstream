---
name: slipstream
description: Builds and guides a personalized, self-updating learning curriculum for staying current in a fast-moving field. On first use it interviews the user, researches the current landscape, and generates a tailored plan. On later use it recommends the next study block sized to available time, runs the session, tracks completion, and keeps the curriculum current against new developments. Use this skill whenever the user wants to catch up on or stay current with a field, build a study plan or learning roadmap, asks "what should I learn next", says "slipstream" or "catch-up session" or "briefing", or wants to track progress through self-directed learning. Trigger it even when the user only describes the goal ("I've fallen behind on AI", "help me get up to speed on X") without naming a plan.
---

# Slipstream

A self-directed learning system for busy professionals who need to catch up on or
stay current with a fast-moving field. It builds a personalized curriculum, guides
the user through it one session at a time, tracks what they have done, and keeps
the plan current as the field changes.

The system is file-based by design. All state lives in three markdown files in the
working folder, not in conversational memory. This makes progress durable,
inspectable, and versionable, and means any fresh session reconstructs full context
by reading the files. Never rely on memory of prior conversations for state; read
the files.

## Requirements and setup

This skill reads and writes files, so it needs a connected working folder (for
example, a Claude Cowork session with a local folder connected, or any environment
with filesystem access). If you cannot read or write files in the current
environment, say so plainly and tell the user the skill needs a connected folder to
work, rather than simulating the files in chat.

The three state files, all in the working folder:

- `PROFILE.md` — who the user is and what they care about. The durable context that
  every other mode reads. Created once during onboarding, edited rarely.
- `PLAN.md` — the curriculum. Generated during onboarding, maintained continuously.
- `PROGRESS.md` — the tracker and changelog. Holds completion state and a log of
  plan changes.

An optional fourth file, `ORG.md`, may be present if an organization has deployed
this skill with shared context (approved tools, tech stack, sponsored resources).
If it exists, read it during onboarding and bias recommendations accordingly. If it
does not exist, ignore it entirely. Never require it.

Templates for all four files are in `templates/`. Read the relevant template before
generating a file so the structure stays consistent.

## Detecting which mode to run

At the start of every invocation, check for the state files:

- **No `PROFILE.md` and no `PLAN.md`** → Onboarding mode. The user is new. Run the
  intake and generate their plan.
- **Files exist, and the user wants to study or asks what is next** → Session mode.
- **Files exist, and the user is discussing the plan itself, surfacing new
  interests, or reacting to developments** → Maintenance mode.

When ambiguous, prefer Session mode, but stay alert for maintenance cues (see
below). One invocation can shift from session to maintenance and back; that is
normal.

---

## Onboarding mode

Goal: produce a genuinely tailored curriculum, not a template with the user's job
title pasted in. The difference is research. A weak intake collects answers and
fills a skill tree. A strong one collects a tight set of answers, then researches
the current landscape for that person's field and role before writing anything, so
the plan reflects what actually exists right now.

### Step 1: Interview

Ask a focused set of questions. Keep it short; interview fatigue kills these. Where
the current conversation already answers a question, do not re-ask it. Prefer
tappable/multiple-choice questions where the environment supports them, with room
for free text on the open-ended ones.

Cover these, adapting the wording to the user's field:

1. **Field and goal.** What do they want to catch up on or stay current with, and
   why now? (A role change, falling behind, a new responsibility.) To avoid the
   blank-page problem, offer a starter list of broad fields alongside an open
   option:

   AI & Machine Learning · Software Engineering · Cybersecurity · Data & Analytics ·
   Cloud & Infrastructure · Product & Design · Marketing & Growth · Finance &
   Investing · Healthcare & Medicine · Law & Regulation · Leadership & Management ·
   Other (describe your field)

   If the environment offers interactive input UI (tappable choices, selectors),
   use it; note that pickers often cap the number of options per question, so a
   list this long may render better as plain text with selectors reserved for the
   shorter questions that follow. In plain-text environments, present it as a
   numbered list. Always include the "Other" path, and treat it as a first-class
   answer, not an edge case.

   A broad pick is an entry point, not an answer. Whatever they choose, drill down
   immediately: which parts of that field, for what purpose? "Cybersecurity" as a
   final answer produces a generic plan; "cloud security posture for a fintech
   platform team" produces a good one. Do not proceed to plan generation on a
   one-word field.

   For the drill-down, offer sub-topic choices rather than a blank question — a
   person who has fallen behind often cannot enumerate what is inside the field
   anymore, and showing them the current map lets them point. But always say
   explicitly that they can describe their focus in their own words instead of
   picking (typing works even when choices are shown, and users will not know that
   unless told). And ask the *purpose* part ("for what role, toward what?") as a
   free-form question, never as a picker — goals cannot be enumerated, and a
   tapped-not-typed purpose is the main way vague plans get generated.
2. **Role and level.** What is their job and seniority, and what are they
   responsible for? This drives depth calibration for everything downstream.
3. **Do vs. know.** Do they need to be able to *do* things (build, ship, operate)
   or to *understand* things (evaluate, decide, converse credibly), or both and in
   what mix? This is the most important question and the one generic tools skip. It
   separates a hands-on builder track from a strategic-fluency track.
4. **Domains, ranked.** Which sub-areas matter most, in priority order? Ranking, not
   just selection — the ranking becomes the relevance filter later.
5. **Existing strengths and rusty areas.** What do they already know well (so the
   plan can skip it) and what is genuinely weak or out of date (so the plan can
   target it)?
6. **Time reality and formats.** How much time can they protect, how consistent is
   it, and which formats work for them (courses, hands-on labs, reading, podcasts,
   video)?
7. **Budget posture.** Free only, willing to pay, or employer-sponsored? Changes
   what gets recommended.

If `ORG.md` exists, weave its context in here (for example, note that certain tools
are the org standard) but do not let it override the user's stated preferences.

### Step 2: Research before generating

Before writing the plan, research the current state of the field against the user's
answers. This is not optional; it is what makes the output better than a static
template. Look up:

- What has changed recently in their priority domains, so the plan reflects the
  present rather than your training cutoff.
- Current, real learning resources — courses, docs, papers, newsletters — that
  match their formats and budget. Verify resources exist and check timing
  (cohort dates, whether something is discontinued) rather than recommending from
  memory.
- Where the field consensus sits, so priorities are defensible.

Flag recommendation confidence honestly. If a resource is well-established, say so;
if it came from a thin source, say that too. A confident plan full of stale or
imaginary courses is the main failure mode to avoid.

### Step 3: Generate the three files

Read the templates first, then write:

- **`PROFILE.md`** — a durable summary of who the user is and what they care about,
  including the ranked domains, the do/know mix, strengths and gaps, time and format
  preferences, and budget posture. Written so a future session with zero prior
  context understands this person from this file alone.
- **`PLAN.md`** — the curriculum. Organize into phases: a rapid rebase to close the
  headline gap, then targeted deep dives in priority order, then an ongoing
  maintenance system (subscriptions, briefings). Where the user wants both doing and
  knowing, run parallel tracks (a builder track and a strategy track) and say so.
  Include specific, researched resources with rough time estimates. **Include an
  explicit "What to deprioritize" section** — half the value is telling the user
  what to skip, since scarce time is the actual constraint. A generator that only
  ever adds is worse than one that also subtracts.
- **`PROGRESS.md`** — the tracker, seeded with one row per plan block, all marked
  not started, plus empty "Open threads" and "Plan changes" sections.

### Step 4: Hand off

Summarize the plan in a few sentences, note the first three concrete actions, and
tell the user how to start a session. When telling them how to invoke this skill,
always call it Slipstream and use the phrase "slipstream" or "what's next"; never
suggest other trigger words. Do not paste the full files into chat; they are on
disk. Point to them.

---

## Session mode

Goal: move the user through one well-chosen block, then record it.

1. **Read `PROGRESS.md` and `PLAN.md` first.** Never ask what the user has already
   done — you have the file. Opening with a status question signals the system is
   not working. If it has been a while since the last session, open with a one-line
   recap of where they left off, not a comment about the gap.

2. **Ask one thing: how much time today.** Offer rough buckets (fifteen minutes, an
   hour, a half-day block). Do not pile on more questions.

3. **Recommend exactly one block, sized to the window.** Not a menu. Pick the
   highest-value next item that fits the time available, say briefly why it is next,
   and begin. If the user wants something else, they will say so.

4. **Run the block according to its type:**
   - *Briefing* — research and synthesize, then deliver a readout filtered for the
     user's context. End with concrete implications for their actual work.
   - *Reading* — the user reads on their own. Prep them on what to look for, then
     debrief and pressure-test afterward.
   - *Course/module* — track enrollment and completion. Debrief after each module
     against their real work.
   - *Lab* — hands-on building. Scope something achievable in the window and work
     through it together.

5. **Pressure-test before marking complete.** Ask the user to explain the concept in
   the context of their own work, not as a quiz but as a real question ("how does
   this change what you'd do on X?"). If the answer is thin, say so and leave the
   block open rather than marking it done.

6. **Update `PROGRESS.md`.** Set status, date, and a short note on what landed or
   what to revisit. Preserve everything already in the file. Then tell the user what
   is queued next so they know where they are.

---

## Maintenance mode

The plan is a living document. You are expected to update it, not just read it. This
is what keeps the curriculum from fossilizing as the field moves.

**Update `PLAN.md` without being asked when:**
- Research or conversation surfaces something better than what is currently listed.
- A listed resource is stale, discontinued, or its timing no longer works.
- The user names a new gap or interest the plan does not cover.
- A development in the field makes a section obsolete or adds a needed one.

**When you update it:**
1. Edit the file directly. Do not ask permission for content updates and do not
   paste the file into chat for the user to save by hand — that is the manual loop
   this skill exists to remove.
2. Append an entry to the "Plan changes" section of `PROGRESS.md`: date, what
   changed, why. One or two lines.
3. If new blocks were added, add matching rows to the `PROGRESS.md` tracker, keeping
   numbering consistent with the plan sections.
4. Tell the user in one sentence what you changed. The changelog is their audit
   trail.

**Ask before editing only when** the change would remove work the user has already
completed, or when it significantly reorders priorities rather than adding material.

**Never rewrite a whole file when a section edit will do.** Preserve the user's notes
and completion history exactly. An agent that regenerates the document each time will
quietly eat the user's progress, and they will not notice until they need it. This is
the single most important safety rule in the skill.

### Keeping current

During sessions, watch for developments relevant to the user's priority domains. If
something material shifts, surface it, and if it warrants a plan change, make one per
the rules above. When a development is worth sharing beyond the user (with a manager,
a team), say so explicitly and offer to help draft that.

---

## Style

Match the user's communication preferences if `PROFILE.md` records any. Absent
that, default to: direct prose, minimal filler, no false encouragement. Be honest
about confidence and tradeoffs. Respect the user's time above all — short and useful
beats long and complete. The user is a capable professional catching up, not a
beginner being onboarded; calibrate depth to the level `PROFILE.md` describes and do
not over-explain things they already know.
