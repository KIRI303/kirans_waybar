# Waybar

My personal Waybar configuration for **Hyprland on Arch Linux**, featuring a minimal dark theme, rounded modules, Nerd Font icons, and essential system information.

## Screenshots

![Waybar](screenshots/waybar.png)

![Waybar 2](screenshots/waybar2.png)

## Features

- Hyprland workspace indicators
- CPU and memory usage
- Clock with an alternate date format
- Wi-Fi signal strength and network status
- Audio volume and mute status
- Battery percentage and charging indicators
- System tray support
- Minimal dark theme with rounded corners
- JetBrainsMono Nerd Font styling

## Repository Structure

```text
waybar/
├── config.jsonc
├── style.css
└── README.md
```

| File           | Purpose                                                       |
| -------------- | ------------------------------------------------------------- |
| `config.jsonc` | Module layout, formatting, update intervals, and interactions |
| `style.css`    | Fonts, colors, spacing, borders, and module appearance        |
| `README.md`    | Installation and configuration guide                          |

## Requirements

- Arch Linux
- Hyprland
- Waybar
- JetBrainsMono Nerd Font
- NetworkManager, with `nm-applet` for the network tray menu
- `pavucontrol` for the audio module's click action

## Installation

### 1. Install the required packages

```bash
sudo pacman -S waybar ttf-jetbrains-mono-nerd networkmanager network-manager-applet pavucontrol
```

Ensure NetworkManager is enabled if you use it to manage Wi-Fi:

```bash
sudo systemctl enable --now NetworkManager
```

### 2. Clone the repository

```bash
git clone https://github.com/KIRI303/waybar.git
cd waybar
```

### 3. Install the configuration

Back up any existing Waybar configuration first:

```bash
mkdir -p ~/.config/waybar

[ ! -f ~/.config/waybar/config.jsonc ] || \
    cp ~/.config/waybar/config.jsonc ~/.config/waybar/config.jsonc.bak

[ ! -f ~/.config/waybar/style.css ] || \
    cp ~/.config/waybar/style.css ~/.config/waybar/style.css.bak
```

Copy the configuration files:

```bash
cp config.jsonc style.css ~/.config/waybar/
```

### 4. Configure Hyprland

Add the following to your Hyprland configuration if Waybar is not already started automatically:

```ini
exec-once = waybar
```

Avoid adding a duplicate startup entry if Waybar is already launched by another script or service.

### 5. Start Waybar

```bash
waybar
```

To restart an existing Waybar process after making changes:

```bash
pkill waybar
waybar &
```

## How It Works

### `config.jsonc`

Waybar uses JSONC, a JSON-compatible format that supports comments, to configure the bar's behavior and layout.

The main options define:

- `position` and `height` — where the bar appears and how tall it is.
- `modules-left`, `modules-center`, and `modules-right` — the order and placement of modules.
- Module-specific settings — formatting, update intervals, and click actions.

For example:

```jsonc
"cpu": {
    "format": " {usage}%",
    "interval": 2
}
```

This displays CPU utilization and refreshes the module every two seconds.

The workspace module should use `hyprland/workspaces` for Hyprland rather than a compositor-specific module for another desktop environment.

### `style.css`

The stylesheet controls the visual appearance of the bar:

- Font family, size, and weight
- Background and text colors
- Padding, margins, and rounded corners
- Active, focused, hovered, and urgent workspace states
- Battery charging and warning colors

The configuration and stylesheet are separate, making it possible to change the appearance without changing module behavior.

## Customization

1. Edit `config.jsonc` to add, remove, or rearrange modules.
2. Change each module's `format` property to adjust its displayed text and icons.
3. Edit `style.css` to customize the theme, typography, spacing, and borders.
4. Restart Waybar to apply changes.

## Troubleshooting

**Waybar does not start:** Run `waybar` in a terminal to inspect errors.

**Icons appear incorrectly:** Verify that JetBrainsMono Nerd Font is installed and that the configured font family is available.

**The Wi-Fi tray icon is missing:** Check that `nm-applet` is running and that `tray` is included in the Waybar configuration.

**Workspace indicators do not work:** Verify that the configuration uses `hyprland/workspaces` and that Waybar is running inside a Hyprland session.

## License

Use and modify this configuration to suit your own setup.
