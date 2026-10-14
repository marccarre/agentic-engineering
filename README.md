# agentic-engineering

As I have been periodically been asked for it, this repository contains examples
reflecting how I use Agentic Engineering, once I have validated one specific
tool or practice, and really included it in my day-to-day workflow.

Hopefully, this can be useful to you!

Cheers,

Marc.

## Cost cutting

### Reduce input tokens with `rtk`

```console
$ brew install rtk
$ rtk init -g

RTK hook registered (global).

  Command:   rtk hook claude
  RTK.md:    $HOME/.claude/RTK.md (awareness: default)
  CLAUDE.md: @RTK.md reference added

Patch existing $HOME/.claude/settings.json? [y/N] y

  settings.json: hook added
  Backup: $HOME/.claude/settings.json.bak
  Restart Claude Code. Test with: git status
  filters:   $HOME/Library/Application Support/rtk/filters.toml (template, edit to add user-global filters)


--- Telemetry ---
RTK collects anonymous usage metrics once per day to improve filters.

  What:    command names (not arguments), token savings, OS, version
  Why:     prioritize filter development for the most-used commands
  Who:     RTK AI Labs, contact@rtk-ai.app
  Rights:  disable anytime with `rtk telemetry disable`,
           request erasure with `rtk telemetry forget`
  Details: https://github.com/rtk-ai/rtk/blob/master/docs/TELEMETRY.md

Enable anonymous telemetry? [y/N]
  Telemetry disabled.
```

### Reduce output tokens with `caveman`

For Claude, with hooks:

```console
$ npx -y github:JuliusBrussee/caveman -- --only claude --with-hooks
🪨 caveman installer
  JuliusBrussee/caveman

→ Claude Code detected
  $ claude plugin marketplace add JuliusBrussee/caveman
✔ Marketplace 'caveman' already on disk — declared in user settings
  $ claude plugin install caveman@caveman
✔ Successfully installed plugin: caveman@caveman (scope: user)
  --with-hooks wires hooks in settings.json alongside the plugin manifest.
  Both will fire on every event. Pass --no-hooks to keep only the plugin path.
  → installing hooks
  installed: $HOME/.claude/hooks/package.json
  installed: $HOME/.claude/hooks/caveman-config.js
  installed: $HOME/.claude/hooks/caveman-parse.js
  installed: $HOME/.claude/hooks/caveman-activate.js
  installed: $HOME/.claude/hooks/caveman-mode-tracker.js
  installed: $HOME/.claude/hooks/caveman-stats.js
  installed: $HOME/.claude/hooks/caveman-statusline.sh
  installed: $HOME/.claude/hooks/caveman-statusline.ps1
  installed: $HOME/.claude/hooks/cavecrew-model-overrides.js
  NOTE: existing statusline detected — caveman badge NOT added.
        See src/hooks/README.md to add the badge to your existing statusline.
  hooks wired in $HOME/.claude/settings.json

  tip: re-run inside a repo with --all (or --with-init) to also write per-repo
       Cursor/Windsurf/Cline/Copilot/AGENTS.md rule files.

🪨 done
  installed:
    • claude
    • claude-hooks

  start any session and say 'caveman mode', or run /caveman in Claude Code
  measure what caveman save you: run /caveman-stats
  verified team savings coming soon — join waitlist: https://caveman.so
  uninstall: npx -y github:JuliusBrussee/caveman -- --uninstall
```

N.B.: Uninstall with the following command:

```console
npx -y github:JuliusBrussee/caveman -- --uninstall
```
