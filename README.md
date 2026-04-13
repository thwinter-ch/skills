# Skills

A growing collection of [Claude Code](https://docs.claude.com/claude-code) skills by [Thomas Winter](https://www.linkedin.com/in/thomaswinter/).

Each skill is a self-contained folder you can drop into `~/.claude/skills/` and Claude will discover it automatically.

## Install

Clone the repo and symlink (or copy) the skills you want:

```bash
git clone https://github.com/thwinter-ch/skills.git ~/skills-repo

# Install one skill
cp -r ~/skills-repo/<skill-name> ~/.claude/skills/

# Or symlink so you get updates with `git pull`
ln -s ~/skills-repo/<skill-name> ~/.claude/skills/<skill-name>
```

Windows users: use `mklink /D` instead of `ln -s`, or just copy the folder.

## Available skills

| Skill | What it does |
|---|---|
| [`scipab`](./scipab) | Structures a data dump into a 7-part SCIPAB decision brief (Situation → Risk) so the reader can jump straight to the decision. |

## How skills work

Every skill is a directory containing a `SKILL.md` with frontmatter that tells Claude when to invoke it. Read [the skill docs](https://docs.claude.com/claude-code/skills) for the full spec.

## License

MIT — use, fork, modify, redistribute. Attribution appreciated, not required.

## Contact

Built by Thomas Winter. Find me on [LinkedIn](https://www.linkedin.com/in/thomaswinter/) or open an issue here.
