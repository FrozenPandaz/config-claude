# Claude Code Configuration

My personal configuration files for Claude Code CLI.

**Note: This setup is specifically tailored for my workflow and preferences.**

## What's Included

- **CLAUDE.md** - Global instructions and preferences
- **settings.json** - Application settings
- **commands/** - Custom commands
- **.gitignore** - Excludes sensitive data like credentials and conversation histories

## Setup

Quick setup from anywhere:
```bash
# Fresh install (will fail if ~/.claude already exists)
git clone https://github.com/FrozenPandaz/config-claude.git ~/.claude

# Install into existing directory
rm -f ~/.claude/settings.json && mkdir -p ~/.claude && cd ~/.claude && git init && git remote add origin https://github.com/FrozenPandaz/config-claude.git && git pull origin main --allow-unrelated-histories
```

Or manual setup:
1. Clone this repository to your `.claude` directory
2. Customize `CLAUDE.md` with your preferences
3. Adjust `settings.json` as needed

## Notes

This configuration emphasizes short, clear communication and stores work notes in `tmp/notes/` for reference.