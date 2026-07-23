# Slipstream

**A Claude Skill that gets you caught up in your field, and keeps you there, without you having to figure out what to learn.**

You fell behind because you were busy doing the actual work. Now the field has moved, you have no idea how far, and every "just read these 40 newsletters" suggestion assumes time you do not have. The real problem is not motivation. It is that deciding what to learn now costs more than the learning itself.

Slipstream removes that overhead. It interviews you about your role and goals, researches what is actually current in your field right now, and generates a learning plan built for your time, your gaps, and your job. Then it guides you through the plan one session at a time, tracks what you finish, and quietly rewrites the plan as the field changes. You show up with whatever time you have. It always knows what is next.

The name comes from cycling: riding in the slipstream of the riders ahead lets you cover the same ground on a fraction of the energy. Same idea here. The route is researched, sequenced, and maintained for you, so your limited time goes into learning instead of deciding what to learn.

*(Not affiliated with any other product named Slipstream. This is an open-source Claude Skill for self-directed learning.)*

## What does Slipstream actually do?

Three things, and it figures out which one you need automatically:

**1. Builds your plan.** On first use, it asks a short set of questions: your field, your role, whether you need hands-on ability or strategic fluency, which areas matter most, what you already know, how much time you really have, and what you are willing to spend. Then it researches the current landscape before writing anything, so your plan reflects courses and resources that exist today, not a template. Every plan includes a "what to skip" section, because protecting your time from tangents is half the value.

**2. Runs your sessions.** Say "slipstream" whenever you have time to learn. It reads your progress, asks one question (how much time do you have right now?), and picks the single best next step that fits that window. Fifteen minutes gets a briefing. A free Saturday gets a deep dive. It checks your understanding before marking anything complete, so finished actually means learned.

**3. Keeps the plan alive.** Fields move. Courses go stale, new tools ship, priorities shift. Slipstream updates your plan on its own as it learns about developments in your field, and logs every change to a changelog you can audit. Mention a new interest in conversation and it gets written into the plan. You never hand-edit anything.

## Who is this for?

Anyone whose field moves faster than their calendar allows. It was built by a technology leader catching up on AI, but nothing in it is tech-specific. It works for:

- Engineers and technical leaders getting current on AI, cloud, or security
- Doctors, nurses, and clinicians tracking new research and practice standards
- Lawyers and compliance professionals following regulatory change
- Marketers, finance professionals, and product managers whose tools keep changing
- Anyone returning from leave, switching roles, or stepping into new responsibility

If you can name the field, Slipstream can build the plan.

## How is this different from courses, newsletters, or corporate training?

**Courses** teach a fixed syllabus to everyone. Slipstream builds a syllabus for exactly one person, and courses become items inside it when they fit.

**Newsletters** tell you what happened. They do not know what you already covered, what you skipped, or what your role makes relevant. Slipstream filters everything through your profile and your progress.

**Corporate learning platforms** personalize a path through a content library an admin bought. Slipstream draws on the open internet, answers to no admin, and rewrites its own curriculum. It is a tutor with a memory, not a course catalog.

**AI chat on its own** forgets you between conversations. Slipstream stores everything in three plain files it maintains for you, so every session picks up exactly where the last one ended. Your plan and progress are readable markdown you own, on your machine, versionable and portable.

## What do I need?

- A paid Claude plan (Pro, Max, Team, or Enterprise). Skills are not available on the free tier.
- The Claude desktop app with Cowork, plus a folder on your computer where Slipstream keeps your three files (your profile, your plan, your progress).
- No coding. Setup is a zip upload and a folder selection.

## How do I install it?

1. Download this repository (green Code button, then Download ZIP) or clone it.
2. Zip the `slipstream` folder itself, so the folder containing `SKILL.md` is the root of the archive. If you downloaded the repo ZIP, extract it first and re-zip just the `slipstream` folder.
3. In the Claude desktop app, open **Customize**, click **+**, choose **Create skill**, and upload the zip.
4. Confirm `slipstream` appears in your skills list and is toggled on.

## How do I use it?

**First run.** Create an empty folder for Slipstream (for example, `Documents/slipstream`). In Claude, switch to Cowork, connect that folder, and say something like "help me build a plan to catch up on my field" or just "slipstream." It will interview you, research, and generate your plan into the folder.

**Learning sessions.** Whenever you have time, connect the folder and say "slipstream" or "what's next." It picks your next block, sized to the time you have.

**Changing the plan.** Just talk to it. "Add that course we discussed." "I want to go deeper on X." "Drop the podcast track." It edits the plan directly and logs the change.

## Can my team or company use it?

Yes. An optional `ORG.md` file lets a team share context, such as your tech stack, approved learning platforms, and how course sponsorship works, so everyone's generated plan leans toward what your organization actually uses. If the file is present, Slipstream reads it during setup; if not, it is ignored. A template is included. Keep it free of anything confidential, and never publish it with real internal data.

## FAQ

**Does it only work for AI or tech topics?**
No. The skill contains no field-specific content. Plan quality is strongest in well-documented fields (technology, medicine, law, finance) because the research step has more to work with, but the machinery is identical for any field.

**Do I need to know how to code?**
No. Installing is uploading a zip. Using it is a conversation.

**Where is my data?**
In three markdown files in a folder you choose on your own computer: `PROFILE.md` (who you are), `PLAN.md` (your curriculum), and `PROGRESS.md` (your tracker and changelog). You can read, edit, back up, or version them like any other files. The skill itself contains no personal data, which is why one skill works for anyone.

**What happens if I disappear for three weeks?**
Nothing breaks. Your next session opens with a one-line recap of where you left off and picks up from there. The plan is designed around inconsistent time, not streaks.

**Will it recommend things that cost money?**
Only within the budget posture you set during the interview. Free-only is a fully supported answer.

**Can it really update its own curriculum?**
Yes, and every change it makes is appended to a changelog in `PROGRESS.md` with the date and reason. It never rewrites your files wholesale, and it never deletes your completed work without asking.

## What's in the repository?

```
slipstream/
├── SKILL.md                        the skill: behavior only, no user data
├── README.md
├── LICENSE
└── templates/
    ├── PROFILE.template.md         who the learner is
    ├── PLAN.template.md            the curriculum structure
    ├── PROGRESS.template.md        tracker + changelog
    └── ORG.template.md             optional shared team context
```

## Known limitations

- Sessions need file access, which means Cowork with a connected folder (or another environment with filesystem access). Plain chat cannot read or write the state files.
- Scheduled or remote runs cannot see files on your computer unless the desktop app is open and the folder is connected. For always-on automation, keep the state files somewhere a remote session can reach.
- Research is only as current as the searches behind it. Slipstream flags its confidence, but verify course dates and prices before spending money.

## License

MIT. See [LICENSE](LICENSE). Built by Jeremy Morris.
