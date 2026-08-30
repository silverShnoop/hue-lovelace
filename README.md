# Hue Lovelace

Custom Lovelace cards for Philips Hue rooms in Home Assistant, built around the
way the lights are actually used: **scenes are the control, brightness is the
occasional adjustment.**

Companion to [`hue_active_scene`](https://github.com/silverShnoop/hue_active_scene),
which exposes the active scene and the adaptive scene's schedule — neither of
which core Hue surfaces.

## `hue-scene-rail`

One row per room, about 46px tall.

- The adaptive all-day scene (Hue's "Golden hours" and friends) is a **toggle**
  at the left, rendered as a gradient across the whole day's palette.
- Every other scene is a **button**, an icon on that scene's own colour.
- Whichever scene is live **expands to show its name**; the rest stay icon-only.
  The expansion *is* the highlight, so the row always reads as "this is what is
  on" without a separate indicator.
- **Dragging the row dims**, and only touches the bulbs the scene left on — not
  the room group, which would drag along every bulb the scene deliberately
  turned off.

Scene colours are read from the schedule sensor's timeslots rather than
configured, so the rail follows whatever the bridge reports.

### Options

| Option | Required | Description |
| --- | --- | --- |
| `type` | yes | `custom:hue-scene-rail` |
| `scenes` | yes | Scene entities. Either an entity id or `{entity, icon, color, name}`. |
| `light` | yes | The room group light, used as the off target and excluded from dimming. |
| `name` | | Row label. Also strips the room prefix from scene names. |
| `area` | | Area to resolve individual bulbs from, for dimming. |
| `lights` | | Explicit bulb list. Wins over `area`. |
| `smart_scene` | | The adaptive scene. Omit for no Auto toggle. |
| `active_scene_sensor` | | `sensor.<room>_active_scene`. Drives the highlight. |
| `schedule_sensor` | | `sensor.<room>_<scene>_schedule`. Supplies scene colours. |
| `auto_label` | | Label on the Auto chip. Default `Auto`. |
| `auto_icon` | | Icon on the Auto chip. Default `mdi:brightness-auto`. |
| `dim` | | Set `false` to disable drag-to-dim. Default `true`. |

### Example

```yaml
type: custom:hue-scene-rail
name: Kitchen
area: kitchen
light: light.kitchen
smart_scene: scene.kitchen_golden_hours_2
active_scene_sensor: sensor.kitchen_active_scene
schedule_sensor: sensor.kitchen_golden_hours_schedule
scenes:
  - { entity: scene.kitchen_arise,      icon: mdi:weather-sunset-up }
  - { entity: scene.kitchen_shine,      icon: mdi:white-balance-sunny }
  - { entity: scene.kitchen_storybook,  icon: mdi:book-open-variant }
  - { entity: scene.kitchen_unwind,     icon: mdi:sofa }
  - { entity: scene.kitchen_sleepy,     icon: mdi:moon-waning-crescent }
  - { entity: scene.kitchen_night_time, icon: mdi:weather-night }
```

### Notes

Hue exposes no "deactivate" service for an adaptive scene, so tapping Auto
while it is on turns the room off. Dropping out of Auto by picking any other
scene works the way the bridge already behaves.

Dimming is relative to where the drag started, so touching the row never jumps
the brightness. Movement under 8px counts as a tap.

## Installing

HACS → three-dot menu → Custom repositories → this repo, category **Lovelace**.
HACS registers the resource itself.

To install by hand instead, copy `dist/hue-lovelace.js` to `config/www/` and add
`/local/hue-lovelace.js` as a dashboard resource of type JavaScript module.

## Adding cards

HACS registers exactly one resource per plugin repository, so every card lives
in `dist/hue-lovelace.js` and registers itself. There is no build step: add the
class, define the element, and append to `window.customCards`.

## Theming

Everything follows the active theme through CSS custom properties. The one
exception is the text and icon colour on a scene chip, which is chosen for
contrast against that scene's own colour — a property of the data, not the
theme.

## Licence

MIT
