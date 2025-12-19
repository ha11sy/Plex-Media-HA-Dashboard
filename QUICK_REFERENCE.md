# Quick Reference Card

## 📁 Files Included

| File | Purpose |
|------|---------|
| `SETUP_GUIDE.md` | Complete installation and configuration guide |
| `media_dashboard_template.yaml` | Dashboard YAML template (partial - see guide for full version) |
| `automation_simple.yaml` | Wake-up automation for devices |
| `QUICK_REFERENCE.md` | This file - quick troubleshooting tips |

## 🔍 Quick Diagnostics

### Check if Plex Integration is Working
1. Go to: **Settings → Devices & Services → Plex**
2. You should see your media players listed
3. Click on a player → Should show current state

### Find Your Entity IDs
**Developer Tools → States** → Search for:
- `media_player.` to find your players
- `sensor.plex` to find Plex sensors
- `calendar.radarr` / `calendar.sonarr` for calendars

### Test Media Player Manually
**Developer Tools → Actions:**
```yaml
service: media_player.turn_on
target:
  entity_id: media_player.YOUR_DEVICE
```

### Get Plex Token
**Browser Console (F12):**
```javascript
localStorage.getItem('myPlexAccessToken')
```
Or from Plex URL when playing media: `X-Plex-Token=...`

## 🎯 Common Issues & Quick Fixes

| Problem | Quick Fix |
|---------|-----------|
| Play button greyed out | Wait 10-15 seconds after selecting room OR enable Instant-On mode on device |
| "Nothing happens" when clicking play | Check Plex token and IP address in YAML |
| Cards not showing | Install custom cards via HACS, clear browser cache |
| Automation not working | Verify `input_select.plex_active_player` exists |
| Wrong library shown | Check `libraryName` exactly matches Plex library name |
| Sensor errors | Comment out sensors you don't have OR install integrations |

## ⚡ Quick Setup Checklist

- [ ] Install all custom cards via HACS
- [ ] Set up Plex integration
- [ ] Get Plex token
- [ ] Create 4 input_select helpers
- [ ] Create 5 navigation scripts
- [ ] Create wake-up automation
- [ ] Copy dashboard template
- [ ] Replace all `YOUR_*` placeholders
- [ ] Test with one player first
- [ ] Add more players if working
- [ ] Configure filters and genres
- [ ] Create Plex Smart Collections (optional)

## 🎨 Customization Quick Tips

### Add More Players
Duplicate this section for each player:
```yaml
- type: custom:button-card
  name: Room Name
  icon: mdi:icon-name
  tap_action:
    action: call-service
    service: input_select.select_option
    data:
      entity_id: input_select.plex_active_player
      option: media_player.your_entity
```

### Add More Genres
Add to `input_select.media_filter_genre` options:
- Comedy
- Drama
- Horror
- Sci-Fi
- Documentary
- etc.

Then create matching Plex Smart Collections or filter sections

### Change Colors
Search for `rgba(...)` in YAML to find color codes:
- `rgba(16, 124, 16, 0.8)` = Green (Player 1)
- `rgba(94, 92, 230, 0.8)` = Purple (Player 2)
- `rgba(255, 159, 9, 0.8)` = Orange (accent)

## 🔗 Useful Commands

### Restart Plex
```yaml
service: button.press
target:
  entity_id: button.plex_restart
```

### Scan Plex Library
```yaml
service: button.press
target:
  entity_id: button.plex_scan_library
```

### Test Player Wake
```yaml
service: media_player.turn_on
target:
  entity_id: media_player.YOUR_DEVICE
```

### Check Device State
**Developer Tools → States** → Search: `media_player.YOUR_DEVICE`

Possible states:
- `idle` = Ready for commands ✅
- `playing` = Currently playing ✅
- `paused` = Paused
- `off` = Device off
- `unavailable` = Not reachable ❌

## 📝 Testing Workflow

1. **Test Plex Integration**
   - Check integration shows players
   - Players show as "idle" or "playing"

2. **Test Input Helpers**
   - Can manually change `input_select.plex_active_player`
   - Scripts exist and run without errors

3. **Test Manual Playback**
   - Use Developer Tools → Actions
   - `media_player.play_media` works

4. **Test Automation**
   - Select a player
   - Device turns on automatically
   - Wait for "idle" state

5. **Test Dashboard**
   - Player selection highlights correctly
   - Plex card loads content
   - Play button works

## 💡 Pro Tips

1. **Use Instant-On Mode** on Xbox/consoles
2. **Keep devices in standby** not fully off
3. **Test one player fully** before adding more
4. **Check entity states frequently** in Developer Tools
5. **Clear browser cache** after YAML changes
6. **Use Plex Smart Collections** for best filtering

## 📞 Getting Help

If you're stuck:
1. Check SETUP_GUIDE.md troubleshooting section
2. Verify all prerequisites are installed
3. Check Home Assistant logs for errors
4. Test components individually (integration, helpers, scripts, automation, dashboard)
5. Ask in Home Assistant community forums with specific error messages

---

**Remember:** This dashboard works best when devices are in standby mode, not fully powered off!
