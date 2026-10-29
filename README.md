# My waybar configs

My personal Linux conifg, featuring two versions of my customized Waybar setup for Hyprland on Arch Linux.

## Screenshots

### Waybar-1 — Original Setup

![Waybar-1](screenshots/waybar.png)

![Waybar-2](screenshots/waybar2.png)

### Waybar-2 — Updated Setup

![Waybar-2 — Additional View](screenshots/waybar2.1.png)

![Waybar-2 — Additional View](screenshots/waybar2.2.png)

## Repository Structure

```text
dotfiles/
├── README.md
├── screenshots/
│   ├── waybar.png
│   ├── waybar2.png
│   ├── waybar2.1.png
│   └── waybar2.2.png
└── waybar/
    ├── waybar-1/
    │   ├── config.jsonc
    │   └── style.css
    └── waybar-2/
        ├── config.jsonc
        └── style.css
```

## Waybar-1 — Previous Configuration

Features:

- Hyprland workspace indicators
- CPU usage and memory usage
- Clock with alternate date display
- Network signal and connection status
- Volume indicator with graphical audio mixer shortcut
- Battery status and charging indicators
- System tray
- Dark theme with rounded modules
- Customized fonts, spacing, and colors

## Waybar-2 — Updated Configuration

Waybar-2 builds on the original design with additional music-player integration.

Additional features:

- Displays the current song title and artist
- Shows a different indicator when music is paused
- Custom Arch Linux icon
- Japanese quote
- Hover tooltip for music information
- Limits track-title length to keep the bar compact
- Lavender accent color for the music module
- Retains the original system monitoring and status modules

## Installation

These instructions assume you have cloned this repository to `~/dotfiles`.

### 1. Install the required packages

On Arch Linux:

```bash
sudo pacman -S waybar git playerctl pavucontrol
```

Packages:

- `waybar` — status bar
- `git` — version control
- `playerctl` — controls compatible media players and reads metadata
- `pavucontrol` — graphical audio mixer

You will also need the fonts used in the configuration if they are not already installed.

### 2. Clone the repository

If you have not cloned the repository yet:

```bash
git clone https://github.com/KIRI303/dotfiles.git
cd dotfiles
```

If the repository already exists on your computer:

```bash
cd ~/dotfiles
git pull
```

### 3. Back up your current Waybar configuration

Before switching versions, back up your current configuration:

```bash
mkdir -p ~/.config/waybar-backup
cp -a ~/.config/waybar/. ~/.config/waybar-backup/
```

This allows you to restore your previous setup if needed.

### 4. Install Waybar-1

Copy the original configuration into Waybar's active configuration directory:

```bash
mkdir -p ~/.config/waybar
cp -r ~/dotfiles/waybar/waybar-1/. ~/.config/waybar/
```

### 5. Install Waybar-2

To switch to the updated configuration instead:

```bash
mkdir -p ~/.config/waybar
cp -r ~/dotfiles/waybar/waybar-2/. ~/.config/waybar/
```

**Important:** Both versions use `~/.config/waybar/` as the active configuration directory. Install one version at a time; your saved versions in the repository remain separate.

### 6. Restart Waybar

```bash
pkill waybar
waybar &
```

If Waybar fails to start, run `waybar` directly in the terminal to inspect any configuration errors.

## Music Player Configuration

Waybar-2 uses the built-in MPRIS module. **Tauon Music Box is optional.** You can use another compatible media player as long as it exposes MPRIS metadata and supports the required playback controls.

Examples include:

- Tauon Music Box
- VLC
- Spotify
- Firefox or Chromium media playback, when MPRIS integration is available
- Other MPRIS-compatible players

### 1. Check whether your player is detected

Open your preferred music player and start playing a track. Then run:

```bash
playerctl -l
```

This lists the media players detected by `playerctl`.

Check the current track's metadata:

```bash
playerctl metadata --format '{{title}} — {{artist}}'
```

If your player is detected and the command displays the current track information, it should be usable with Waybar's MPRIS module.

### 2. Configure Waybar for your preferred player

Open the active Waybar configuration:

```bash
nano ~/.config/waybar/config.jsonc
```

Find the `mpris` section. For example, to use VLC:

```jsonc
"mpris": {
    "player": "vlc",
    "format": "♫ {title} — {artist}",
    "format-paused": "Ⅱ {title} — {artist}",
    "max-length": 35,
    "tooltip": true,
    "on-click": "play-pause",
    "on-click-middle": "previous",
    "on-click-right": "next"
},
```

Replace `"vlc"` with the player name detected by `playerctl -l`. For example, use `"spotify"` if that is the detected name for Spotify.

**Note:** The player name can differ from the application's display name. Use the name reported by your system.

### 3. Automatically follow the active player

If you want Waybar to follow the active compatible media player rather than a specific application, use `playerctld`.

Start the daemon:

```bash
playerctld daemon
```

Then change the player setting in the MPRIS configuration to:

```jsonc
"player": "playerctld",
```

This allows `playerctl` to track the most recently active player. If you prefer to display only one specific player, keep its name in the configuration instead.

### 4. Browser playback

Browser media playback may work when the browser exposes MPRIS metadata. If your browser does not appear in `playerctl -l`, check whether its media-session integration is available and enabled.

### 5. Restart and test

```bash
pkill waybar
waybar &
```

Start or resume playback and check that the song title and artist appear. If the module remains empty, test `playerctl metadata` first to determine whether the player is exposing its metadata correctly.

## Customization

- Edit `config.jsonc` to change modules, displayed information, and click actions.
- Edit `style.css` to customize colors, spacing, fonts, and rounded corners.
- Modify the MPRIS configuration to select a different compatible music player.
- Add screenshots to the `screenshots/` directory and update the image paths in this README when changing filenames.

## Updating the Repository

After modifying configurations, documentation, or screenshots:

```bash
cd ~/dotfiles
git add README.md waybar/ screenshots/
git commit -m "Update Waybar configurations and screenshots"
git push
```

## Notes

- The repository stores both Waybar configurations separately.
- Copying a version into `~/.config/waybar/` makes it the active configuration.
- Music controls require a compatible player and available MPRIS metadata.
- Fonts used by the configurations must be installed separately if they are not already available.
- Back up existing configuration files before replacing them.
