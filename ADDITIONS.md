# Additions in this fork

This is a fork of [daniel3303/ClaudeCodeStatusLine](https://github.com/daniel3303/ClaudeCodeStatusLine)
(MIT). All credit for the statusline itself goes upstream. This fork adds three segments on top.

| Segment | What it shows | Requires |
|---|---|---|
| `[CAVEMAN:ULTRA]` | The active [caveman](https://github.com/JuliusBrussee/caveman) mode, read from `~/.claude/.caveman-active` | caveman plugin |
| `rtk 90% (34.5m saved)` | Tokens filtered by [rtk](https://github.com/rtk-ai/rtk), via `rtk gain -f json` | `rtk`, `jq` |
| line wrapping | Wraps onto a second line when `$COLUMNS` is too narrow, instead of truncating | nothing |

Every segment degrades quietly: if `rtk` or the caveman flag file is missing, that part is simply
not rendered. Nothing errors, nothing blocks the prompt.

## Notes on the implementation

- The caveman badge rejects symlinked flag files, caps the read at 64 bytes, and strips the value
  to `a-z0-9-` before printing it. A status line renders untrusted file contents into your
  terminal every couple of seconds, so it is worth being strict.
- `rtk gain -f json` is fast (~10ms), so it runs inline rather than being cached like the usage
  API call.
- Wrapping measures the *printed* width by stripping ANSI escape text first, so colour codes do
  not count toward the line length.

## Install

```bash
git clone https://github.com/lncch/almoayyad-claude-statusline.git ~/.claude/statusline
chmod +x ~/.claude/statusline/statusline.sh
```

Then add this to `~/.claude/settings.json`:

```json
{
  "statusLine": {
    "type": "command",
    "command": "~/.claude/statusline/statusline.sh",
    "refreshInterval": 2
  }
}
```

Restart Claude Code. See the upstream [INSTALL.md](INSTALL.md) for options, colours, and the
Windows PowerShell version.

### Optional extras

```bash
brew install rtk        # enables the rtk savings segment
```

Install the [caveman plugin](https://github.com/JuliusBrussee/caveman) to enable the mode badge.

## Licence

MIT, unchanged from upstream. See [LICENSE](LICENSE).
