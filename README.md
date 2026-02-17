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

1. Open **System Settings → Keyboard → Shortcuts**
2. Click **Add New → Command or Script**
3. In the command field, enter the full path to the script followed by the window class and launch command:
   ```
   /home/youruser/.local/bin/run-or-cycle.sh brave-browser brave
   ```
4. Click **Add custom shortcut** and press your desired key combination

That's it. The same shortcut will launch the app, focus it, or cycle between its windows.

### Example shortcuts

| App | Command |
|-----|---------|
| Brave | `/home/youruser/.local/bin/run-or-cycle.sh brave-browser brave` |
| Konsole | `/home/youruser/.local/bin/run-or-cycle.sh konsole konsole` |
| Dolphin | `/home/youruser/.local/bin/run-or-cycle.sh dolphin dolphin` |
| Firefox | `/home/youruser/.local/bin/run-or-cycle.sh firefox firefox` |
| VS Code | `/home/youruser/.local/bin/run-or-cycle.sh code code` |

### Finding the window class for any app

To find the correct class name for an app you want to add:

```bash
# Focus the app window, then run:
kdotool getactivewindow
# Returns something like: {4bb44a33-a5d6-4cca-9bb9-10bc9a3ec0f5}

# Search by app name to verify:
kdotool search --class <app-name>
```

### Hyprland

If you use Hyprland instead of KDE, add binds to your config:

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
