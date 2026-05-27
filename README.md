# opencode-codex-usage

Minimal `opencode` TUI plugin that shows live ChatGPT Codex usage in the prompt area.

It reads your existing local `opencode` OpenAI auth and fetches usage from the same backend Codex uses.

## What it shows

- 5h usage
- weekly usage
- time until the 5h window resets
- time until the weekly window resets
- badge shown only when the active model/provider is OpenAI/Codex-related
- percent values colored with a readable pastel Jet colormap, from blue at low usage to red near the limit

Example:

```text
Plus 5h 81% (54m) | wk 13% (3d)
```

## Install

1. Clone this repo anywhere stable.

```bash
git clone https://github.com/5TuX/opencode-codex-usage.git ~/opencode-codex-usage
```

2. Install dependencies.

```bash
cd ~/opencode-codex-usage
npm install
```

3. Point `opencode` TUI config at the plugin.

Create or update `~/.config/opencode/tui.json`:

```json
{
  "$schema": "https://opencode.ai/tui.json",
  "plugin": [
    "/absolute/path/to/opencode-codex-usage/plugins/codex-usage.tsx"
  ]
}
```

4. Quit and restart `opencode`.

## Notes

- Requires an existing `opencode` OpenAI login.
- Reads auth from `~/.local/share/opencode/auth.json`.
- Reads current model selection from `~/.local/state/opencode/model.json` when available.
- Refreshes every 60 seconds.
- Registers a command: `Refresh Codex usage`.
- Hidden for non-Codex providers (for example Copilot).
- Provider/model visibility is decided from the currently selected model first, then recent session history as fallback.
- Visibility is refreshed every second and on TUI command/session changes so switching provider/model updates the badge without sending a message.

## Synced Device Workflow

For the shared multi-device setup, edit this repo copy of the plugin:

```text
~/Documents/gdrive-shared/opencode/opencode-codex-usage/plugins/codex-usage.tsx
```

Each device should keep `~/.config/opencode/plugins/` as a real local directory and hardlink or copy the plugin file into it. The local `tui.json` should load the local file, not the synced repo path directly:

```json
{
  "$schema": "https://opencode.ai/tui.json",
  "plugin": [
    "/home/YOU/.config/opencode/plugins/codex-usage.tsx"
  ]
}
```

After editing the repo file:

1. Let the synced folder update on each device.
2. Recreate the hardlink if the sync tool replaced the file.
3. Restart `opencode` on each device.

Linux hardlink example:

```bash
ln -f ~/Documents/gdrive-shared/opencode/opencode-codex-usage/plugins/codex-usage.tsx ~/.config/opencode/plugins/codex-usage.tsx
```

If dependencies changed, run this on each device:

```bash
cd ~/.config/opencode
npm install
```

## Minimal Roadmap

- show remaining instead of used
- add a footer/sidebar variant
