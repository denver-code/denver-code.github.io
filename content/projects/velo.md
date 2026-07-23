+++
title = "Velo"
description = "The ultimate car tracking app designed to bridge the gap between heavy maintenance logs and rapid entry for daily drivers. A core sub-project of Veylor HQ."
weight = 0
[extra]
local_image = "/projects/velo_full.jpg"
+++

# Velo — The Ultimate Car Tracking App

**Part of the Veylor HQ Ecosystem | B2C Automotive Intelligence**

Velo is designed to bridge the gap between heavy, spreadsheet-like maintenance logs and the rapid-entry needs of a daily driver. Built with Veylor's local-first and anti-bloat principles, Velo gives vehicle owners complete control over their maintenance telemetry, cost of ownership, and preventative service schedules.

I was using LubeLogger for a long time, but the overall UX was not good and I wanted certain features to work differently and be more mobile-friendly. So I decided to build my own app, which is now called Velo. It is a mobile-first app, but can be ran on desktop, web or etc thanks to Flutter;
Currently it's under Veylor-hq umbrella, I'm using it personally along with a few friends, but at some point in future, it would be released to public as a separate app that is Open Source, can be used with API provided by me or completely locally, downloadable straight from website, play store/app store or used as a PWA. It is a very simple app, but it has a lot of potential to grow and become a full-fledged car tracking app with more features and capabilities.

<div style="text-align: center; margin: 1.5em 0;">
    <img alt="Velo App Interface Screenshot" async src="/projects/velo_full.jpg" style="max-width: 380px; width: 100%; border-radius: 12px; box-shadow: 0 4px 20px rgba(0,0,0,0.3);" />
</div>

---

## Features & Capabilities

### Refueling & Fuel Economy
- **Rapid Entry**: Optimised for "at-the-pump" logging with minimal taps and odometer tracking.
- **Efficiency Analytics**: Automated calculation of L/100km, MPG, and cost-per-mile metrics.
- **Fuel Stations**: Geolocation-aware tagging to track which fuel brands deliver superior performance.

### Mods, Service & Maintenance
- **Detailed Service Logs**: Track oil changes, filter replacements, and major repairs with high-resolution photo attachments.
- **Preventative Reminders**: Set time- or mileage-based triggers for upcoming maintenance (e.g., *"Change oil every 10k miles"* or *"every 6 months"*).
- **Recurring Expenses**: Track non-mechanical costs like Insurance, Road Tax, MOT, Fines, or parking fees.
- **Parts Catalog**: Store specific part numbers (spark plug codes, oil filter models) for instant reference.

### Advanced Automotive Intelligence
- **Cost of Ownership**: Visual breakdown of total spend per vehicle, categorized by fuel, service, and upgrades.
- **Sentinel Planner**: A complex maintenance forecast system predicting part wear based on driving habits.

### Mobile-First & Connectivity
- **Lock Screen Widgets**: One-tap access to log refuels or check remaining mileage until next service.
- **Telegram Sync**: Receive maintenance reminders and weekly spend summaries directly via a Telegram bot.
- **Offline Mode**: Log data seamlessly underground or in remote areas; automatic background sync when reconnected.

### Data & Portability
- **LubeLogger Import/Export**: Fully compatible CSV/JSON data structures for seamless migration.
- **Multi-Vehicle Support**: Manage an entire fleet from a single account with granular per-vehicle permission sharing.

---

## Architectural Context

Velo forms the consumer (B2C) arm of Veylor's automotive platform, sharing a unified backend infrastructure with **eGarage** (B2B Garage Management). All external notifications and user telemetry pass through [Veylor Relay](https://github.com/veylor-hq/relay) to preserve PII privacy and zero-trust security compliance.
