# OneUI

A modern, lightweight NUI library for FiveM — notifications, dialogs, menus, progress bars, skill checks, and text UI prompts. Built as a single shared resource that other scripts depend on.

---

## Features

- **Notifications** — Toast-style notifications with configurable position, duration, type (info/success/warning/error), title, and description.
- **Text UI** — World-space prompts (e.g. "[E] Open Door") that appear when a player is near an interactable. Supports custom keys and auto-dismiss.
- **Input Dialogs** — Multi-field modal dialogs with text, number, select, checkbox, and textarea inputs. Returns submitted values or `null` on cancel.
- **Alert Dialogs** — Confirmation modals with customizable labels and optional cancel button.
- **Context Menus** — Navigation-style menus with arrow-key support, scrollable values, checkboxes, and mouse toggle.
- **Progress Bars** — Linear and circular progress indicators with cancel support and completion callbacks.
- **Skill Checks** — Timing-based skill check minigames with adjustable difficulty (easy/medium/hard or custom zone size).
- **NUI Focus Management** — Clean helpers for `SetNuiFocus` / `SetNuiFocusKeepInput` with automatic state restoration.

---

## Installation

1. Download the `OneUI` folder.
2. Place it in your server's `resources/` directory.
3. Add to your `server.cfg`:

```
ensure OneUI
```

4. Start OneUI **before** any resource that depends on it.

---

## Usage (for developers)

Any resource that wants to use OneUI needs these lines in its `fxmanifest.lua`:

```lua
dependency 'OneUI'

shared_script '@OneUI/init.lua'
```

This boots the global `lib` table with all OneUI functions. Example usage:

```lua
-- Notification
lib.notify({
    type = 'success',
    title = 'Saved',
    description = 'Your settings have been saved.',
    duration = 4000,
})

-- Text UI prompt
lib.showTextUI('[E] Open Door', {
    icon = 'door',
})

-- Input dialog
local input = lib.inputDialog({
    heading = 'Enter Player Name',
    rows = {
        { type = 'text', label = 'Name', placeholder = 'John Doe' },
        { type = 'number', label = 'Age', min = 0, max = 120 },
    },
})
if input then
    local name, age = input[1], input[2]
    print(name, age)
end

-- Alert dialog
lib.alertDialog({
    header = 'Confirm Delete',
    content = 'Are you sure you want to delete this file?',
    labels = { confirm = 'Delete', cancel = 'Cancel' },
})

-- Progress bar
lib.progressBar({
    label = 'Picking lock...',
    duration = 5000,
})
```

---

## API Reference

| Function | Description |
|----------|-------------|
| `lib.notify(data)` | Show a notification |
| `lib.showTextUI(text, options)` | Show a world-space prompt |
| `lib.hideTextUI()` | Hide the current text UI prompt |
| `lib.isTextUIOpen()` | Returns `true` if text UI is open |
| `lib.inputDialog(data)` | Open a multi-field input dialog |
| `lib.alertDialog(data)` | Open a confirmation alert |
| `lib.registerMenu(data, cb)` | Register a context menu |
| `lib.showMenu(id, startIndex)` | Show a registered menu |
| `lib.hideMenu(onExit)` | Hide the current menu |
| `lib.progressBar(data)` | Show a linear progress bar |
| `lib.progressCircle(data)` | Show a circular progress bar |
| `lib.cancelProgress()` | Cancel the active progress bar |
| `lib.skillCheck(data)` | Start a skill check minigame |
| `lib.setNuiFocus(allowInput, showCursor)` | Set NUI focus state |
| `lib.resetNuiFocus()` | Reset NUI focus to default |

---

## Configuration

Edit `resource/settings.lua`:

```lua
return {
    notification_position = 'top-right',  -- top-left, top-right, bottom-left, bottom-right
    notification_duration = 5000,         -- default duration in ms
}
```

---

## Requirements

- FiveM server
- Lua 5.4 (`lua54 'yes'` in fxmanifest)

---

## Author

**OneSystems**

OneUI is the foundation library for the OneSystems script ecosystem — including [OneCCTV]([#](https://github.com/OneSystems-LL/OneCCTV-0.9.0)).
