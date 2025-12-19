# Plex Media Dashboard for Home Assistant
Complete setup guide for creating a beautiful, functional Plex media dashboard in Home Assistant.

## 📸 Features

- **Multi-Device Player Selection** - Choose where to play content (Living Room, Bedroom, etc.)
- **Auto-Wake Devices** - Automatically wakes up selected device before playback
- **Visual Now Playing** - Full artwork with playback controls and progress
- **Movies & TV Shows** - Browse your complete Plex library
- **Filters & Genres** - Filter by ratings (G, PG, PG-13, R, TV-MA) and genres
- **Featured Collections** - Trending Now, New HD Releases
- **Quick Actions** - Restart Plex, Scan Library, Clear History, Update content
- **Upcoming Content** - Calendar view of upcoming movies and TV episodes
- **Responsive Design** - Works on desktop, tablet, and mobile

## 📋 Prerequisites

### Required Integrations:
1. **Plex Media Server** - Installed via Home Assistant integrations
2. **Media Players** - At least one Plex-capable device (Chromecast, Xbox, Smart TV, etc.)
3. **Custom Cards** (Install via HACS):
   - `button-card`
   - `mini-media-player`
   - `plex-meets-homeassistant`
   - `mushroom`
   - `atomic-calendar-revive`
   - `layout-card`
   - `vertical-stack-in-card`

### Optional Integrations:
- **Radarr** - For upcoming movies calendar
- **Sonarr** - For upcoming TV episodes calendar
- **Overseerr** - For pending/processing requests tracking
- **SABnzbd** - For download queue monitoring

## 🔧 Installation Steps

### Step 1: Install Custom Cards via HACS

1. Open **HACS** in Home Assistant
2. Go to **Frontend**
3. Click **"+ Explore & Download Repositories"**
4. Search for and install each card:
   - `button-card`
   - `mini-media-player`
   - `plex-meets-homeassistant`
   - `mushroom`
   - `atomic-calendar-revive`
   - `layout-card`
   - `vertical-stack-in-card`
5. Restart Home Assistant

### Step 2: Set Up Plex Integration

1. Go to **Settings → Devices & Services**
2. Click **"+ Add Integration"**
3. Search for **"Plex"**
4. Follow the authentication flow
5. Select your Plex server
6. Your media players should appear as entities

### Step 3: Get Your Plex Token

You need your Plex authentication token for the dashboard:

**Method 1: From Plex Web App**
1. Open your Plex Web App: `http://YOUR_PLEX_IP:32400/web`
2. Press `F12` to open Developer Console
3. Go to **Console** tab
4. Type: `localStorage.getItem('myPlexAccessToken')`
5. Copy the token (without quotes)

**Method 2: From a Plex URL**
1. Play any item in Plex Web
2. Click the **three dots** → **Get Info**
3. Click **View XML**
4. Look for `X-Plex-Token=` in the URL
5. Copy the token value

### Step 4: Create Input Helpers

Create these input helpers in Home Assistant:

**Settings → Devices & Services → Helpers → Create Helper**

1. **Input Select: Plex Active Player**
   - Name: `Plex Active Player`
   - Entity ID: `input_select.plex_active_player`
   - Options:
     - `media_player.living_room_tv` (your first device)
     - `media_player.bedroom_tv` (your second device)
     - Add more as needed

2. **Input Select: Media Dashboard View**
   - Name: `Media Dashboard View`
   - Entity ID: `input_select.media_dashboard_view`
   - Options:
     - `Movies`
     - `TV Shows`

3. **Input Select: Media Sidebar Mode**
   - Name: `Media Sidebar Mode`
   - Entity ID: `input_select.media_sidebar_mode`
   - Options:
     - `Main`
     - `Filters`

4. **Input Select: Media Filter Genre**
   - Name: `Media Filter Genre`
   - Entity ID: `input_select.media_filter_genre`
   - Options:
     - `None`
     - `Trending Movies Now`
     - `New HD Releases`
     - `G`
     - `PG`
     - `PG-13`
     - `R`
     - `TV-MA`
     - `Unrated`
     - `Action`
     - `Comedy`
     - `Drama`
     - `Horror`
     - `Sci-Fi`
     - Add more genres as needed

### Step 5: Create Navigation Scripts

Create these scripts in **Settings → Automations & Scenes → Scripts**:

**Script 1: Media Nav Movies Clean**
```yaml
alias: Media Nav Movies Clean
sequence:
  - service: input_select.select_option
    data:
      entity_id: input_select.media_dashboard_view
      option: Movies
  - service: input_select.select_option
    data:
      entity_id: input_select.media_filter_genre
      option: None
  - service: input_select.select_option
    data:
      entity_id: input_select.media_sidebar_mode
      option: Main
mode: single
```

**Script 2: Media Nav Movies Menu**
```yaml
alias: Media Nav Movies Menu
sequence:
  - service: input_select.select_option
    data:
      entity_id: input_select.media_dashboard_view
      option: Movies
  - service: input_select.select_option
    data:
      entity_id: input_select.media_sidebar_mode
      option: Filters
mode: single
```

**Script 3: Media Nav TV Clean**
```yaml
alias: Media Nav TV Clean
sequence:
  - service: input_select.select_option
    data:
      entity_id: input_select.media_dashboard_view
      option: TV Shows
  - service: input_select.select_option
    data:
      entity_id: input_select.media_filter_genre
      option: None
  - service: input_select.select_option
    data:
      entity_id: input_select.media_sidebar_mode
      option: Main
mode: single
```

**Script 4: Media Nav TV Menu**
```yaml
alias: Media Nav TV Menu
sequence:
  - service: input_select.select_option
    data:
      entity_id: input_select.media_dashboard_view
      option: TV Shows
  - service: input_select.select_option
    data:
      entity_id: input_select.media_sidebar_mode
      option: Filters
mode: single
```

**Script 5: Media Menu Back**
```yaml
alias: Media Menu Back
sequence:
  - service: input_select.select_option
    data:
      entity_id: input_select.media_sidebar_mode
      option: Main
mode: single
```

### Step 6: Create Wake-Up Automation

This automation wakes up the selected device before playback:

**Settings → Automations & Scenes → Create Automation → Edit in YAML**

```yaml
alias: Plex - Wake Selected Player
description: Wakes up device and waits until it's ready for Plex playback
trigger:
  - platform: state
    entity_id: input_select.plex_active_player
condition: []
action:
  - service: media_player.turn_on
    target:
      entity_id: "{{ trigger.to_state.state }}"
  - wait_template: "{{ is_state(trigger.to_state.state, 'idle') or is_state(trigger.to_state.state, 'playing') }}"
    timeout:
      seconds: 30
    continue_on_timeout: true
  - delay:
      seconds: 2
mode: restart
```

### Step 7: Add the Dashboard

1. Go to **Settings → Dashboards**
2. Click **"+ Add Dashboard"**
3. Choose **"New dashboard from scratch"**
4. Name it: `Media`
5. Click **"Take Control"** (if using YAML mode)
6. Click the **three dots** → **"Edit in YAML"**
7. Copy the entire contents of `media_dashboard_template.yaml` (provided separately)
8. **Replace the placeholders** with your information (see Customization section below)
9. Save

## 🎨 Customization

### Required Changes in the Dashboard YAML:

1. **Plex Token** (Line varies - search for `token:`)
   - Replace: `YOUR_PLEX_TOKEN_HERE`
   - With: Your actual Plex token from Step 3

2. **Plex Server IP** (Line varies - search for `ip:`)
   - Replace: `YOUR_PLEX_IP`
   - With: Your Plex server IP (e.g., `192.168.1.100`)

3. **Media Player Entities** (Multiple locations)
   - Replace: `media_player.living_room_tv`
   - With: Your actual media player entity ID
   - Replace: `media_player.bedroom_tv`
   - With: Your second media player entity ID
   - Add more players if needed

4. **Player Names** (In the "Play On" section)
   - Replace: `Living Room` / `Bedroom`
   - With: Your room names

5. **Plex Library Names** (Multiple locations - search for `libraryName:`)
   - Default: `Movies`, `TV Shows`
   - Change if your libraries have different names
   - Add custom collections like `🔥 Trending Movies Now` if you have them

6. **Optional Sensor Entities** (Top status bar)
   - `sensor.plex_active_streams` - Shows active Plex streams
   - `sensor.overseerr_pending_requests` - Overseerr pending requests
   - `sensor.overseerr_approved_requests` - Overseerr processing requests
   - `sensor.radarr_movies` - Total movie count
   - `sensor.sonarr_shows` - Total TV show count
   - `sensor.sabnzb_queue_count` - Download queue count
   - **Comment out or remove** sensors you don't have

7. **Calendar Entities** (Bottom right sidebar)
   - `calendar.radarr` - Upcoming movies
   - `calendar.sonarr` - Upcoming episodes
   - **Comment out or remove** if you don't have Radarr/Sonarr

## 📝 Entity ID Reference

Find your entity IDs in **Settings → Devices & Services → Plex**:

| Type | Example Entity ID | Where to Find |
|------|-------------------|---------------|
| Media Player (Chromecast) | `media_player.living_room_tv` | Plex integration devices |
| Media Player (Xbox) | `media_player.xbox` | Plex integration devices |
| Media Player (Smart TV) | `media_player.bedroom_tv` | Plex integration devices |
| Plex Streams | `sensor.plex_SERVER_NAME` | Plex integration sensors |
| Radarr Movies | `sensor.radarr_movies` | Radarr integration |
| Radarr Calendar | `calendar.radarr` | Radarr integration |
| Sonarr Shows | `sensor.sonarr_shows` | Sonarr integration |
| Sonarr Calendar | `calendar.sonarr` | Sonarr integration |

## 🎯 Plex Library Configuration

### Setting Up Smart Collections (Optional)

To use features like "Trending Movies Now" or "New HD Releases", create Smart Collections in Plex:

1. Open **Plex Web** → Go to your **Movies** library
2. Click **"+"** → **"Collection"**
3. Name it: `🔥 Trending Movies Now`
4. Add filters:
   - **Release Year** is **2024** or **2025**
   - **Sort by** → **Release Date** (Newest first)
5. Save

Repeat for other collections you want (Action Movies, Horror, etc.)

### Matching Library Names

Your `libraryName` in the YAML **must exactly match** your Plex library names:
- Check Plex Web → Settings → Libraries
- Use exact names including spaces and special characters
- Examples: `Movies`, `TV Shows`, `4K Movies`, `Kids Movies`

## 🐛 Troubleshooting

### Play Button Greyed Out

**Cause:** Device is not in "idle" or "playing" state

**Solutions:**
1. Check device is powered on
2. For Xbox: Enable **Instant-On** mode (Settings → Power mode)
3. For Chromecast/Smart TVs: Wake up the device manually first
4. Wait 10-15 seconds after selecting a room
5. Check entity state in **Developer Tools → States**

### "Nothing Happens" When Clicking Play

**Cause:** Usually a Plex token or IP address issue

**Solutions:**
1. Verify Plex token is correct (see Step 3)
2. Test Plex is accessible: `http://YOUR_PLEX_IP:32400/web`
3. Check media player entity is "idle" not "unavailable"
4. Try manually via **Developer Tools → Actions**:
   ```yaml
   service: media_player.play_media
   target:
     entity_id: media_player.YOUR_DEVICE
   data:
     media_content_type: plex
     media_content_id: '{"library_name": "Movies"}'
   ```

### Cards Not Showing / Errors

**Cause:** Missing custom cards

**Solutions:**
1. Verify all custom cards are installed via HACS
2. Clear browser cache (Ctrl + Shift + R)
3. Check browser console for errors (F12)
4. Restart Home Assistant

### Automation Not Working

**Cause:** Template or entity ID mismatch

**Solutions:**
1. Check automation in **Settings → Automations** for errors
2. Verify `input_select.plex_active_player` exists
3. Test manually: **Developer Tools → Actions** → `media_player.turn_on`
4. Check automation traces for failures

### Sensors Missing (Optional Features)

**Cause:** Integration not installed or configured

**Solutions:**
1. Comment out sensors you don't have in the YAML
2. Install missing integrations (Radarr, Sonarr, Overseerr, SABnzbd)
3. Or replace with generic text/numbers

## 💡 Tips & Best Practices

1. **Enable Instant-On for Xbox**
   - Xbox Settings → General → Power mode → **Instant-on**
   - Keeps Plex app in memory for instant wake

2. **Keep Devices in Standby**
   - Don't fully power off devices
   - Use standby/sleep mode for faster wake-up

3. **Test One Device First**
   - Get one media player working before adding more
   - Easier to troubleshoot

4. **Use Generic Icons Initially**
   - Don't worry about custom icons until everything works
   - Focus on functionality first

5. **Check Entity States**
   - Use **Developer Tools → States** frequently
   - Helps identify issues quickly

## 📚 Additional Resources

- [Plex Meets Home Assistant Card](https://github.com/JurajNyiri/PlexMeetsHomeAssistant) - Full documentation
- [Home Assistant Plex Integration](https://www.home-assistant.io/integrations/plex/)
- [Button Card Documentation](https://github.com/custom-cards/button-card)
- [Mini Media Player Documentation](https://github.com/kalkih/mini-media-player)

## 🤝 Credits

Dashboard design and implementation inspired by modern streaming interfaces with iOS dark mode aesthetics.

## 📄 License

Feel free to use, modify, and share this dashboard configuration!

---

**Questions or Issues?**
Check the troubleshooting section above or consult the Home Assistant community forums.
