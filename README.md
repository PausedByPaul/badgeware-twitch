# Badgeware Twitch

Show off your Twitch streamer stats on a wearable badge — a brilliant way to engage with your community IRL! Whether you're at a meetup, convention, or just hanging out, your badge keeps your audience in the loop with live follower counts, latest subscribers, cheers, and more.

Supports both the **Pimoroni Badger 2350** (e-ink) and **Pimoroni Tufty 2350** (colour LCD).

## Features

- **Follower & subscriber counts** — always visible at a glance
- **Latest follower** — see who just followed you
- **Latest subscriber** — including tier info
- **Latest gifted sub** — gifter name and gift count
- **Latest cheer** — bit amount and cheerer name
- **Live status indicator** — shows when you're streaming
- **Profile avatar** — displayed on the Tufty 2350's colour screen
- **Auto-rotating views** — cycle through stats hands-free (Only on Tufty2350)
- **Battery & charging indicator** — so you know when to top up
- **Smart caching** — loads instantly from cache, refreshes in the background
- **Low battery mode** — disables animations below 20% to extend battery life

## Requirements

- **Pimoroni Badger 2350** and/or **Pimoroni Tufty 2350**
- **Badge firmware v2.0.1** — both Badger 2350 and Tufty 2350 must be running firmware version **2.0.1** or later
  - [Badger 2350 firmware](https://github.com/pimoroni/badger2350) 
  - [Tufty 2350 firmware](https://github.com/pimoroni/tufty2350)
- A WiFi network for the badge to connect to
- A **Twitch UUID** from the Badge API service (see below)

## Sign Up for the Badge API

Rather than running your own backend, anyone can sign up for the hosted API service at:

> **https://badge.pausedbypaul.net**

Sign up, link your Twitch account, and you'll receive a **UUID** that your badge uses to fetch your streamer stats. No self-hosting required.

## Installation

### 1. Flash the firmware

Make sure your badge is running firmware **v2.0.1** or later. Follow the instructions in the respective firmware repositories for your device:

- [Badger 2350 firmware repo](https://github.com/pimoroni/badger2350)
- [Tufty 2350 firmware repo](https://github.com/pimoroni/tufty2350)

### 2. Copy the Twitch app to your badge

Connect your badge via USB (it will appear as a mass storage device).

**For Badger 2350:**

Copy the contents of the `badger2350/` folder from this repo into:

```
apps/twitch/
```

Your badge storage should look like:

```
apps/twitch/__init__.py
```

**For Tufty 2350:**

Copy the contents of the `tufty2350/twitch/` folder from this repo into:

```
apps/twitch/
```

Your badge storage should look like:

```
apps/twitch/__init__.py
```

### 3. Configure secrets.py

Edit the `secrets.py` file in the root of your badge's filesystem and add your WiFi credentials and Twitch UUID:

```python
WIFI_SSID = "YourNetworkName"
WIFI_PASSWORD = "YourPassword"
TWITCH_UUID = "your-uuid-from-badge-api"
```

### 4. Launch the app

Eject the mass storage device and select **Twitch** from the app menu. On first launch it will connect to WiFi, fetch your stats, and cache them locally.

## Controls

### Badger 2350

| Button | Action |
|--------|--------|
| UP / DOWN | Navigate between pages |
| A (hold) | Refresh data from API |
| HOME | Return to menu |

### Tufty 2350

| Button | Action |
|--------|--------|
| UP / DOWN | Cycle through views |
| A + C (hold 2s) | Force WiFi refresh (clears cache) |
| HOME | Return to menu |

## Display Views

### Badger 2350 (e-ink)

Pages cycle through your stats on the 296×128 monochrome e-ink display:

- **Overview** — display name, broadcaster type, follower & subscriber counts
- **Latest follower** — who just followed
- **Latest subscriber** — name and tier
- **Latest gifted sub** — gifter and count
- **Latest cheer** — cheerer and bit amount

### Tufty 2350 (colour LCD)

The colour display shows richer views with your profile avatar, Twitch-purple themed UI, and animated background:

| View | Content |
|------|---------|
| Avatar + Counts | Profile picture with follower & subscriber totals |
| Latest Follower | Total followers and latest follower name |
| Latest Sub | Total subs and latest subscriber name |
| Latest Gift | Gift count and gifter name |
| Latest Cheer | Bit amount and cheerer name |

Views are dynamically enabled based on your broadcaster type (Affiliate/Partner) and your badge configuration from the API.

## How It Works

1. On startup, the app loads cached data from the badge's local storage for an instant display
2. It connects to WiFi and fetches your latest stats from the Badge API.
3. The response includes your streamer data and a `badge_config` object that controls which views are shown and auto-scroll timing
4. Data is cached locally so the badge works offline and starts up quickly
5. WiFi is disconnected after fetching to conserve battery

## Project Structure

```
badgeware-twitch/
├── README.md
├── LICENSE
├── badger2350/
│   └── twitch/
│       └── __init__.py      # Badger 2350 twitch app
└── tufty2350/
    └── twitch/
        └── __init__.py      # Tufty 2350 twitch app
```

## License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.
