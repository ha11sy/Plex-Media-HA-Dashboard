# 🎬 Plex Media Dashboard for Home Assistant

A beautiful, functional Plex media dashboard for Home Assistant with multi-device support, automatic wake-up, and visual now-playing cards.

![Dashboard Preview](https://img.shields.io/badge/Home%20Assistant-Compatible-blue)
![Plex](https://img.shields.io/badge/Plex-Integrated-orange)
![HACS](https://img.shields.io/badge/HACS-Required-green)

## ✨ Features

### 🎯 Multi-Device Control
- Select which device to play content on (TV, Xbox, Chromecast, etc.)
- Visual player selection with color-coded highlighting
- Automatic device wake-up before playback

### 🎥 Content Browsing
- Browse your complete Plex Movies and TV Shows libraries
- Filter by ratings (G, PG, PG-13, R, TV-MA, Unrated)
- Filter by genres (Action, Comedy, Drama, Horror, etc.)
- Featured collections (Trending Now, New HD Releases)

### 🎵 Now Playing
- Full-cover artwork display
- Real-time playback controls (play/pause, next, volume)
- Progress bar with time tracking
- Friendly room names ("Playing in Living Room")

### 📅 Upcoming Content
- Calendar view of upcoming movie releases (Radarr)
- Calendar view of upcoming TV episodes (Sonarr)

### ⚡ Quick Actions
- Restart Plex server
- Scan library for new content
- Clear download history
- Update Radarr/Sonarr content
- Open Overseerr

## 📦 What's Included

| File | Description |
|------|-------------|
| **SETUP_GUIDE.md** | Complete step-by-step installation guide |
| **media_dashboard_template.yaml** | Template dashboard configuration |
| **automation_simple.yaml** | Device wake-up automation |
| **QUICK_REFERENCE.md** | Quick troubleshooting & tips |
| **README.md** | This file |

## 🚀 Quick Start

### Prerequisites
- Home Assistant (2024.1 or newer)
- HACS installed
- Plex Media Server
- At least one Plex-capable media player

### Installation (5 Steps)

1. **Install Custom Cards** (via HACS)
   - button-card
   - mini-media-player
   - plex-meets-homeassistant
   - mushroom
   - atomic-calendar-revive
   - layout-card
   - vertical-stack-in-card

2. **Set Up Plex Integration**
   - Settings → Integrations → Add Plex
   - Authenticate with your Plex account

3. **Create Input Helpers**
   - 4 input_select helpers (see SETUP_GUIDE.md)

4. **Create Scripts**
   - 5 navigation scripts (see SETUP_GUIDE.md)

5. **Install Dashboard**
   - Copy template → Replace placeholders → Save

**See `SETUP_GUIDE.md` for detailed instructions!**

## 📸 Screenshots

### Main Dashboard
- Multi-device player selection
- Movies and TV Shows browsing
- Filter by rating and genre

### Now Playing
- Full-cover artwork
- Playback controls
- Progress tracking

### Calendar View
- Upcoming movies
- Upcoming TV episodes

## 🎨 Customization

### Easy Customizations
- Change room names and icons
- Add more media players
- Add more genres and filters
- Change color schemes
- Add more Plex libraries

### Required Changes
You must customize these before the dashboard will work:
1. **Plex Token** - Your authentication token
2. **Plex IP** - Your server's IP address
3. **Entity IDs** - Your media player entities
4. **Library Names** - Your Plex library names

**See comments in `media_dashboard_template.yaml` for all customization points!**

## 🐛 Troubleshooting

### Common Issues

**Play button greyed out?**
- Wait 10-15 seconds after selecting room
- Enable Instant-On mode on Xbox/consoles
- Keep devices in standby, not fully powered off

**Nothing happens when clicking play?**
- Verify Plex token is correct
- Check Plex server IP is reachable
- Test manually via Developer Tools

**Cards not showing?**
- Install all custom cards via HACS
- Clear browser cache (Ctrl + Shift + R)
- Restart Home Assistant

**See `SETUP_GUIDE.md` for complete troubleshooting guide!**

## 📚 Documentation

- **[Setup Guide](SETUP_GUIDE.md)** - Complete installation instructions
- **[Quick Reference](QUICK_REFERENCE.md)** - Fast troubleshooting tips
- **[Template YAML](media_dashboard_template.yaml)** - Annotated dashboard code
- **[Automation](automation_simple.yaml)** - Wake-up automation

## 💡 Tips for Best Experience

1. **Enable Instant-On mode** on Xbox/consoles
2. **Keep devices in standby** not fully off
3. **Create Plex Smart Collections** for featured content
4. **Test with one device first** before adding more
5. **Use friendly room names** that everyone understands

## 🤝 Contributing

Found a bug? Have a suggestion? Feel free to:
- Open an issue
- Submit a pull request
- Share your customizations

## 📄 License

Free to use, modify, and share!

## 🙏 Credits

- Home Assistant community
- Plex Meets Home Assistant card developer
- Custom card developers (Button Card, Mini Media Player, Mushroom, etc.)

## ⭐ Support

If you find this useful:
- Star the repository
- Share with the Home Assistant community
- Show off your customizations!

---

**Ready to get started?** 

👉 **Open [SETUP_GUIDE.md](SETUP_GUIDE.md) for step-by-step instructions!**

---

*Built with ❤️ for the Home Assistant community*
