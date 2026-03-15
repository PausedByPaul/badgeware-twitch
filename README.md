# Badgeware Twitch

Show off your Twitch streamer stats on a wearable badge - a brilliant way to engage with your community IRL! Whether you're at a meetup, convention, or just hanging out, your badge keeps your audience in the loop with live follower counts, latest subscribers, cheers, and more.

Supports both the **Pimoroni Badger 2350** (e-ink) and **Pimoroni Tufty 2350** (colour LCD).

## Features

- **Follower & subscriber counts** — always visible at a glance
- **Latest follower** — see who just followed you
- **Latest subscriber** — including tier info
- **Latest gifted sub** — gifter name and gift count
- **Latest cheer** — bit amount and cheerer name
- **Live status indicator** — shows when you're streaming
- **Profile avatar** — displayed on the Tufty 2350's colour screen
- **Auto-rotating views** — cycle through stats hands-free (Only for intervals of 30s+ on **Badger 2350**)
- **Battery & charging indicator** — so you know when to top up
- **Smart caching** — loads instantly from cache, upon request by pressing A+C (hold) on Tufty 2350 or A (hold) on Badger 2350
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

If you want to host your own, you will need to adapt the code to point to your server, and respond in the following JSON format:

```json
{
  "uuid": "ccede6b1-ff22-4e47-92db-e30ee8242b3d",
  "user_id": "57779122",
  "handle": "pausedbypaul",
  "display_name": "PausedByPaul",
  "broadcaster_type": "affiliate",
  "profile_image_url": "https://static-cdn.jtvnw.net/jtv_user_pictures/1bf744de-b361-4a5d-8f57-6d48047f2210-profile_image-300x300.png",
  "is_live": false,
  "follower_count": 249,
  "subscriber_count": 3,
  "latest_follower": "awesome_follower",
  "latest_follower_time": "2026-03-09T20:06:00Z",
  "last_cheerer": "cheer_master",
  "last_cheer_amount": 20,
  "last_cheer_time": "2026-01-31T23:52:47Z",
  "latest_subscriber": "great_subscriber",
  "latest_subscriber_tier": "1000",
  "latest_subscriber_time": "2026-03-09T19:53:02Z",
  "latest_sub_gifter": "generous_gifter",
  "latest_sub_gift_count": 5,
  "latest_sub_gift_time": null,
  "badge_config": {
    "auto_scroll": 30,
    "show_latest_sub": true,
    "show_latest_follower": true,
    "show_latest_gifted_sub": true,
    "show_latest_cheer": true
  },
  "last_badge_connect": "2026-03-15T23:21:16Z",
  "created_at": "2026-01-31T23:35:41Z",
  "updated_at": "2026-03-15T23:21:48Z"
}
```

Not all of these fields are available direct from the Twitch Helix API, so my hosted service aggregates data from the Helix API and Twitch EventSub webhooks to provide a comprehensive dataset for the badge.

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
