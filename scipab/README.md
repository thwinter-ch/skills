# scipab

Structure a messy data dump into a **SCIPAB** decision brief — Situation, Complication, Implication, Position, Action, Benefit, Risk.

Based on [Mandel Communications' SCIPAB framework](https://www.mandel.com/scipab-messaging-tool), with **Risk** added as a 7th section so the downside of the proposed action is always explicit.

## When it triggers

- "scipab", "/scipab", "frame this as scipab", "structure this in scipab"
- When you ask for a decision brief, executive summary, or issue report
- Whenever Claude is about to surface a problem and needs a green light

## Why use it

Because reading three paragraphs of investigation prose to find out *what the model wants you to do* is the worst part of working with AI agents. SCIPAB forces the agent to lead with the decision and the action, then back-fill the context.

## Install

```bash
# clone the parent repo
git clone https://github.com/thwinter-ch/skills.git ~/skills-repo

# copy this skill into your Claude skills dir
cp -r ~/skills-repo/scipab ~/.claude/skills/

# or symlink so `git pull` keeps it updated
ln -s ~/skills-repo/scipab ~/.claude/skills/scipab
```

Windows: `mklink /D %USERPROFILE%\.claude\skills\scipab "%USERPROFILE%\skills-repo\scipab"` or just copy the folder.

## License

MIT. See repo root.
