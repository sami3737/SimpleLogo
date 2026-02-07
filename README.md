# SimpleLogo

Display a custom logo on player screens with support for animated slideshows.

## Features

- Display custom logos from image URLs
- Create GIF-like animations by rotating through multiple images
- Fully customizable position and appearance
- Permission-based visibility control
- Player toggle command to show/hide the logo
- Saves player preferences across server restarts

## Dependencies

**Required:**
- [ImageLibrary](https://umod.org/plugins/image-library) - Must be installed for this plugin to work

## Installation

1. Install ImageLibrary plugin
2. Place `SimpleLogo.cs` in your `oxide/plugins` folder
3. Plugin will generate a default configuration file on first load

## Configuration

The plugin creates a configuration file at `oxide/config/SimpleLogo.json`:
```json
{
  "UI": {
    "BackgroundMainColor": "0 0 0 0",
    "BackgroundMainURL": [
      "http://i.imgur.com/KVmbhyB.png"
    ],
    "GUIAnchorMax": "0.15 0.1",
    "GUIAnchorMin": "0.01 0.02",
    "IntervalBetweenImage": 30
  }
}
```

### Configuration Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `BackgroundMainColor` | String (RGBA) | `"0 0 0 0"` | Background color in RGBA format (0-1 range). `"0 0 0 0"` = transparent |
| `BackgroundMainURL` | Array | `["http://..."]` | List of image URLs. Multiple URLs create a slideshow effect |
| `GUIAnchorMin` | String | `"0.01 0.02"` | Bottom-left anchor point (X Y coordinates, 0-1 range) |
| `GUIAnchorMax` | String | `"0.15 0.1"` | Top-right anchor point (X Y coordinates, 0-1 range) |
| `IntervalBetweenImage` | Integer | `30` | Seconds between image transitions (only applies with multiple images) |

### Position Examples

**Bottom Left (default):**
```json
"GUIAnchorMin": "0.01 0.02",
"GUIAnchorMax": "0.15 0.1"
```

**Top Right:**
```json
"GUIAnchorMin": "0.85 0.90",
"GUIAnchorMax": "0.99 0.98"
```

**Center:**
```json
"GUIAnchorMin": "0.45 0.45",
"GUIAnchorMax": "0.55 0.55"
```

### Creating a Slideshow

To create an animated effect, add multiple image URLs:
```json
"BackgroundMainURL": [
  "https://i.imgur.com/image1.png",
  "https://i.imgur.com/image2.png",
  "https://i.imgur.com/image3.png"
],
"IntervalBetweenImage": 5
```

This will rotate through the images every 5 seconds.

## Permissions

### simplelogo.display
**Default:** Not granted
```
oxide.grant group default simplelogo.display
```
Players need this permission to see the logo. Players without this permission won't see anything.

### simplelogo.nodisplay
**Default:** Not granted
```
oxide.grant user PlayerName simplelogo.nodisplay
```
Overrides the display permission. Use this to hide the logo from specific players (e.g., streamers who don't want it in recordings).

**Note:** `simplelogo.nodisplay` takes priority over `simplelogo.display`

## Commands

### /sl
**Permission:** None required  
**Description:** Toggle logo visibility for yourself
```
/sl
```

Your preference is saved and persists across server restarts.

**Example output:**
```
Logo hidden
Logo displayed
```

## Image Requirements

- **Format:** PNG, JPG, or any format supported by ImageLibrary
- **Hosting:** Images must be hosted on a publicly accessible URL
- **Recommended size:** 256x256 pixels or smaller for best performance
- **Transparency:** PNG format recommended for logos with transparency

## Troubleshooting

### Logo not appearing?

1. **Check ImageLibrary is installed:**
```
   oxide.plugins
```
   You should see `ImageLibrary` in the list.

2. **Check permissions:**
```
   oxide.show user YourName simplelogo.display
```

3. **Check if you toggled it off:**
   Type `/sl` to toggle visibility.

4. **Check console for errors:**
   Look for messages like:
   - `"ImageLibrary isn't loaded !"`
   - `"No url registered !"`
   - `"Empty URL at index X"`

### Images not loading?

- Verify URLs are accessible (test in browser)
- Check ImageLibrary console for download errors
- Wait a few seconds after server start for images to cache

### Slideshow not working?

- Ensure you have multiple URLs in `BackgroundMainURL`
- Check `IntervalBetweenImage` is set to a positive number
- Reload the plugin: `oxide.reload SimpleLogo`

## Developer Notes

### Data Storage

Player preferences are stored in `oxide/data/SimpleLogo.json`:
```json
{
  "76561198012345678": true,
  "76561198087654321": false
}
```
- `true` = logo hidden
- `false` = logo displayed

### Hooks Used

- `OnServerInitialized()` - Initialize plugin
- `OnPlayerConnected()` - Show logo to connecting players
- `Unload()` - Cleanup and save data

## Credits

**Author:** Sami37  
**Version:** 1.2.9  

## Support

For issues or suggestions, please visit the plugin support thread on umod.org.

---

**License:** This plugin is provided as-is without warranty.
