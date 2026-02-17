# kde-run-or-cycle

A minimal script for **KDE Plasma (Wayland)** that lets you bind a single keyboard shortcut to:

1. **Launch** an app if it's not running
2. **Focus** its window if it is
3. **Cycle** between multiple windows of the same app

Uses [kdotool](https://github.com/jinliu/kdotool) for Wayland-native window management.

## Dependencies

- KDE Plasma 5 or 6 (Wayland session)
- [kdotool](https://github.com/jinliu/kdotool)

### Install kdotool

```bash
# Arch / CachyOS
pacman -S kdotool

# Or build from source — see https://github.com/jinliu/kdotool
```

## Install

```bash
# Clone the repo
git clone https://github.com/ImJustDoingMyPart/kde-run-or-cycle.git

# Copy to your local bin (or anywhere in your $PATH)
cp kde-run-or-cycle/run-or-cycle.sh ~/.local/bin/
chmod +x ~/.local/bin/run-or-cycle.sh
```

## Usage

```bash
run-or-cycle.sh <window-class> <command>
```

- `window-class` — the window class name to match (used by `kdotool search --class`)
- `command` — the command to launch the app if no window exists

### Examples

| App | Command |
|-----|---------|
| Brave | `run-or-cycle.sh brave-browser brave` |
| Konsole | `run-or-cycle.sh konsole konsole` |
| Dolphin | `run-or-cycle.sh dolphin dolphin` |
| Firefox | `run-or-cycle.sh firefox firefox` |
| VS Code | `run-or-cycle.sh code code` |

### Finding the window class

To find the correct class name for any app:

```bash
# Focus the app window, then run:
kdotool getactivewindow
# Returns something like: {4bb44a33-a5d6-4cca-9bb9-10bc9a3ec0f5}

# Try searching by app name:
kdotool search --class <app-name>
```

## Setting up keyboard shortcuts

### KDE System Settings

1. Go to **System Settings → Shortcuts → Custom Shortcuts**
2. Add a new shortcut (or edit an existing one)
3. Set the **Command** to: `run-or-cycle.sh brave-browser brave`
4. Assign your preferred key combination

### Hyprland

```ini
bind = $mainMod, B, exec, ~/.local/bin/run-or-cycle.sh brave-browser brave
bind = $mainMod, Return, exec, ~/.local/bin/run-or-cycle.sh konsole konsole
```

## How it works

1. Searches for all windows matching the given class via `kdotool search --class`
2. If none found → launches the app with `setsid` (detached from the script)
3. If windows exist → finds the currently active window and activates the **next** one in the list
4. Wraps around to the first window when reaching the end

## License

[MIT](LICENSE)
