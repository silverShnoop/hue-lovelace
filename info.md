# Hue Lovelace

Custom Lovelace cards for Philips Hue rooms, built around scenes rather than
brightness.

## Cards

- **`hue-scene-rail`** — one row per room: a toggle for the adaptive all-day
  scene, a colour-coded button per scene, and drag-to-dim that only moves the
  bulbs the scene left on.

## Requires

The [`hue_active_scene`](https://github.com/silverShnoop/hue_active_scene)
integration, which supplies the active-scene and schedule sensors these cards
read.

## Setup

Install through HACS, then add the card to a dashboard. HACS registers the
resource for you.
