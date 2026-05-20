# pi-config

Personal [pi](https://github.com/earendil-works/pi) coding agent configuration.

## Structure

```
pi-config/
├── README.md
├── .gitignore
└── agent/
    ├── AGENTS.md           — Agent discovery config
    ├── settings.json       — Global pi settings
    ├── keybindings.json    — Keyboard shortcuts
    ├── caveman.json        — Caveman mode preferences
    ├── models.json         — Model provider config
    ├── git/
    │   └── .gitignore      — Git-specific ignores
    ├── npm/               — Pi extension packages (auto-installed)
    ├── agents/             — Custom agent definitions
    ├── prompts/            — Build prompt templates
    ├── themes/             — UI theme files
    └── extensions/         — Extension configs
```

## Setup

1. **Backup** existing config:

   ```sh
   mv ~/.pi/agent ~/.pi/agent.bak
   ```

2. **Symlink** this repo into place:

   ```sh
   ln -s ./agent ~/.pi/agent
   ```

3. **Verify**:
   Pi auto-installs packages from `settings.json` on startup.
   ```sh
   pi doctor
   ```
