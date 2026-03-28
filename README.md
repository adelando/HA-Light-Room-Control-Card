# Room Light Card

A custom [Home Assistant](https://www.home-assistant.io/) Lovelace card for controlling light groups with individual per-light fine controls, colour temperature, RGB colour swatches, and configurable scroll or chevron overflow handling.
![CARD](https://i.imgur.com/gosmUfF.jpeg)
-----

## Features

- **Master toggle button** —
  - Tap/click to open group-wide fine controls,
  - Long-press to toggle all lights on/off
- **Individual light buttons** —
  - Tap/click to open per-light controls,
  - Long-press to toggle that light
- **Per-light fine controls**
  — Brightness slider,
  - colour temperature slider,
  - Warm/Soft/Cool presets, and
  - RGB colour swatches
- **Dynamic temperature slider** —
  - switches to a rainbow gradient when an RGB colour is selected, reverts to warm-to-cool when a temperature preset is chosen
- **Individual light card overflow**
  - Scrolling overflow — horizontal scrolling row with fade edge indicators
  - Chevron overflow — paginated view with prev/next arrows and a page indicator pill
- **Fully themeable** — configure card background, button colours, icon colours, and text colours
- **MDI icon support**
  — any `mdi:` icon for the master button and individual light buttons
- **Per-light icon overrides** — give each light its own icon
- **Works with all light types** — colour temp only, RGB, RGBWW, and brightness-only bulbs

-----

## Installation

### HACS (recommended)

1. Open HACS in your Home Assistant instance
1. Go to **Frontend**
1. Click the three-dot menu in the top right and choose **Custom repositories**
1. Add `https://github.com/adelando/HA-Light-Room-Control-Card` with category **Dashboard**
1. Find **Room Light Card** in the list and click **Download**
1. Restart Home Assistant

### Manual

1. Download `room-light-card.js` from the [latest release](https://github.com/adelando/HA-Light-Room-Control-Card/releases)
1. Copy it to `/config/www/room-light-card.js` (create the `www` folder if it does not exist)
1. Go to **Settings → Dashboards → Resources**
1. Click **Add Resource**
1. Set the URL to `/local/room-light-card.js` and the type to **JavaScript module**
1. Hard refresh your browser (`Ctrl+Shift+R` / `Cmd+Shift+R`)

> **Note:** On some installations the config directory is `/homeassistant/` rather than `/config/`. If `/local/room-light-card.js` returns a 404, try placing the file in `/homeassistant/www/` instead. Confirm the file is reachable by navigating to `http://YOUR-HA-IP:8123/local/room-light-card.js` in your browser.

-----

## Configuration

Add a **Manual card** to your dashboard with the following YAML:
![Card](https://i.imgur.com/XEKzj9f.jpeg)

```yaml
type: custom:room-light-card
name: Side Lights
master_entity: light.side_deck_group
lights:
  - entity: light.side_1
    name: Light 1
  - entity: light.side_2
    name: Light 2
```

### Full example
![Example2](https://i.imgur.com/3bpGJor.jpeg)
```yaml
type: custom:room-light-card
name: Front Lights
master_entity: light.front_yard_group
master_icon: mdi:power
light_icon: mdi:lightbulb
icon_color_on: "#1a1a1a"
icon_color_off: "#888888"
btn_color_on: "#f5c842"
btn_color_off: "#2a2a2a"
card_background: "rgba(0,0,0,0.8)"
text_primary: "#e8e8e8"
text_secondary: "#666666"
overflow: chevron
page_size: 4
lights:
  - entity: light.front_1
    name: Front 1
    icon: mdi:outdoor-lamp
  - entity: light.front_2
    name: Front 2
    icon: mdi:outdoor-lamp
  - entity: light.front_3
    name: Front 3
    icon: mdi:outdoor-lamp
  - entity: light.front_4
    name: Front 4
    icon: mdi:outdoor-lamp
  - entity: light.front_5
    name: Front 5
    icon: mdi:outdoor-lamp
  - entity: light.front_lights
    name: All Front
    icon: mdi:outdoor-lamp
```

-----

## Options

### Card options

|Option           |Type                |Default                 |Description                                                                                                                                                |
|-----------------|--------------------|------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
|`name`           |string              |`Lights`                |Room or group label shown at the top of the card                                                                                                           |
|`master_entity`  |string              |—                       |Entity ID of the light group. The master button controls this entity. If omitted, the master button toggles all listed lights individually                 |
|`overflow`       |`scroll` | `chevron`|`scroll`                |How to handle more lights than fit in the row. `scroll` enables horizontal scrolling with fade indicators. `chevron` paginates with prev/next arrow buttons|
|`page_size`      |number              |`4`                     |Number of lights shown per page in chevron mode                                                                                                            |
|`master_icon`    |string              |`mdi:lightbulb-group`   |MDI icon for the master button                                                                                                                             |
|`light_icon`     |string              |`mdi:lightbulb`         |Default MDI icon for all individual light buttons                                                                                                          |
|`btn_color_on`   |string              |`#f5c842`               |Button background colour when the light is on                                                                                                              |
|`btn_color_off`  |string              |`#2a2a2a`               |Button background colour when the light is off                                                                                                             |
|`icon_color_on`  |string              |`#1a1a1a`               |Icon colour when the light is on                                                                                                                           |
|`icon_color_off` |string              |`#888888`               |Icon colour when the light is off                                                                                                                          |
|`card_background`|string              |`#141414`               |Card background. Accepts any CSS colour value including `rgba()` for transparency or `var(--card-background-color)` for the HA theme colour                |
|`text_primary`   |string              |`rgba(255,255,255,0.85)`|Primary text colour — used for light names, slider values, and panel titles                                                                                |
|`text_secondary` |string              |`rgba(255,255,255,0.35)`|Secondary text colour — used for the room name, state text, slider labels, separators, and dividers                                                        |
|`lights`         |list                |**required**            |List of light entities to show (see below)                                                                                                                 |

### Light options

Each entry in the `lights` list supports:

|Option  |Type  |Default          |Description                              |
|--------|------|-----------------|-----------------------------------------|
|`entity`|string|**required**     |Entity ID of the light                   |
|`name`  |string|entity ID        |Label shown below the button             |
|`icon`  |string|card `light_icon`|MDI icon override for this specific light|

-----

## Interactions

|Action                                    |Result                                                       |
|------------------------------------------|-------------------------------------------------------------|
|Tap master button                         |Opens group-wide brightness, temperature, and colour controls|
|Long-press master button (500ms)          |Toggles the entire group on or off                           |
|Tap individual light button               |Opens per-light fine controls                                |
|Long-press individual light button (500ms)|Toggles that light on or off                                 |
|Drag brightness slider                    |Adjusts brightness for that light                            |
|Drag temperature slider                   |Adjusts colour temperature (warm left, cool right)           |
|Tap Warm / Soft / Cool preset             |Sets colour temperature and syncs the slider                 |
|Tap RGB colour swatch                     |Sets RGB colour and switches slider to rainbow mode          |
|Tap page indicator pill                   |Advances to next page in chevron mode (wraps to first)       |

-----

## Light type support

|Light capability   |Brightness|Temp slider|Warm/Soft/Cool|RGB swatches|
|-------------------|:--------:|:---------:|:------------:|:----------:|
|Brightness only    |Yes       |—          |—             |—           |
|Colour temp only   |Yes       |Yes        |Yes           |—           |
|RGB                |Yes       |—          |—             |Yes         |
|RGBWW / full colour|Yes       |Yes        |Yes           |Yes         |

Colour temperature support is detected automatically from the light’s `supported_color_modes`, `min_mireds`, `max_mireds`, or `color_temp` attributes — so most bulbs are detected correctly even if their integration reports modes inconsistently.

-----

## Tips

- Use `card_background: var(--card-background-color)` to match your current HA theme automatically
- For light backgrounds set `text_primary` and `text_secondary` to dark colours (e.g. `#1a1a1a` and `#777777`)
- The master button in chevron mode uses the same long-press toggle as individual lights — hold for half a second to turn everything off without opening the controls panel
- Per-light icon overrides are useful for mixed groups — for example `mdi:floor-lamp` for floor lamps and `mdi:outdoor-lamp` for garden lights in the same card

-----

## Troubleshooting

**“Custom element not found: room-light-card”**
The JS file is not being loaded. Check:

1. The file exists at the correct path and is accessible at `http://YOUR-HA-IP:8123/local/room-light-card.js`
1. The resource is registered under **Settings → Dashboards → Resources** with type **JavaScript module** (not plain JavaScript)
1. You have done a hard refresh after registering the resource (`Ctrl+Shift+R` / `Cmd+Shift+R`)

**Card loads but changes are not reflected after updating the file**
Append a version query string to the resource URL to bust the cache — e.g. `/local/room-light-card.js?v=2`. Increment the number each time you update the file.

**Temperature slider or presets not showing for a bulb**
The card detects temperature support from the light’s attributes. If your bulb supports colour temperature but the controls are missing, check that the entity has `min_mireds` and `max_mireds` attributes in Home Assistant’s developer tools (Settings → Developer Tools → States).

-----

## Contributing

Pull requests and issues are welcome. Please open an issue before submitting a large change so we can discuss it first.

-----

## License

MIT License — see <LICENSE> for details.
