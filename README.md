# SwayRider

An open-source motorcycle routing platform built with Go microservices, featuring route planning, turn-by-turn navigation, offline maps, and group riding capabilities.

## Features

- **Route Planning** — Multi-modal routing with customizable preferences (highway vs scenic, toll avoidance, ferry avoidance)
- **Turn-by-Turn Navigation** — Real-time GPS navigation with voice guidance and lane assistance
- **Offline Maps** — Downloadable vector map tiles for offline and low-connectivity use
- **Geocoding & Search** — Address and place search with multi-region geocoding backend
- **Points of Interest** — Restaurants, fuel stations, scenic viewpoints, rest areas, repair shops
- **Route Sharing** — Community hub for sharing, rating, and discovering routes
- **Group Riding** — Real-time group coordination with shared routes, participant monitoring, and stop/fuel requests

## Tech Stack

| Layer | Technology |
|-------|------------|
| Backend | Go 1.26, gRPC + Protocol Buffers |
| Database | PostgreSQL |
| Routing Engine | Valhalla |
| Geocoding | Pelias (Elasticsearch) |
| Map Rendering | MapLibre with custom vector tiles |
| Mobile | Kotlin, Jetpack Compose, Clean Architecture |
| Data Pipeline | Python 3.12+, Tippecanoe, Osmium |
| Infrastructure | Docker Compose (layered) |

## Architecture

```
┌─────────────────────────────────────────────────┐
│              Mobile Clients                      │
│         Android (Compose)  ·  iOS (KMP)         │
└───────────────────────┬─────────────────────────┘
                        │ REST / gRPC
┌───────────────────────▼───────────────────────┐
│            Backend Services (Go)                 │
├────────────┬──────────┬────────────────────────┤
│ AuthSvc   │ MailSvc  │ RouterSvc              │
│ JWT auth  │ SMTP    │ Multi-region routing   │
├───────────┼─────────┼─────────────────────────┤
│ RegionSvc │ SearchSvc│ TilesSvc             │
│ Spatial  │ Pelias  │ Vector tiles (MVT) │
└───────────┴─────────┴─────────────────────────┘
                        │ gRPC
┌───────────────────────▼───────────────────────┐
│               Data Layer                       │
│  PostgreSQL  ·  Valhalla  ·  Elasticsearch    │
└────────────────────────────────────────────────┘
```

## Repositories

| Repository | Purpose |
|------------|---------|
| [protos](https://github.com/SwayRider/protos) | gRPC Protocol Buffer definitions |
| [grpcclients](https://github.com/SwayRider/grpcclients) | Go gRPC client wrappers |
| [swlib](https://github.com/SwayRider/swlib) | Go shared library (bootstrap, auth, logging) |
| [authservice](https://github.com/SwayRider/authservice) | User authentication & JWT management |
| [mailservice](https://github.com/SwayRider/mailservice) | Transactional email |
| [regionservice](https://github.com/SwayRider/regionservice) | Geographic region queries |
| [routerservice](https://github.com/SwayRider/routerservice) | Multi-modal routing via Valhalla |
| [searchservice](https://github.com/SwayRider/searchservice) | Geocoding via Pelias |
| [tilesservice](https://github.com/SwayRider/tilesservice) | Vector tile serving |
| [swctl](https://github.com/SwayRider/swctl) | Management CLI |
| [mobile](https://github.com/SwayRider/mobile) | Android app (Kotlin/Jetpack Compose) |
| [data-pipeline](https://github.com/SwayRider/data-pipeline) | OSM → Valhalla/Pelias/tiles pipeline |
| [infra](https://github.com/SwayRider/infra) | Docker Compose, deployment scripts |

## License

SwayRider is open-source software licensed under the **GNU Affero General Public License v3 (AGPL-3.0)**.

See [LICENSE](https://github.com/SwayRider/swayrider/blob/main/LICENSE) for details.

## Contributing

Contributions are welcome. To get started:

1. **Fork** the repository you want to work on
2. **Clone** your fork locally:
   ```bash
   git clone git@github.com:YOUR_USERNAME/<repo>.git
   ```
3. **Create** a feature branch:
   ```bash
   git checkout -b feature/your-feature-name
   ```
4. **Make** your changes and commit with clear messages
5. **Push** to your fork and open a **Pull Request**

For an overview of the full monorepo structure and setup instructions, see the [swayrider/README.md](https://github.com/SwayRider/swayrider/blob/main/README.md).

## Quick Start

### Backend Development

```bash
# Clone the Go modules you need
git clone git@github.com:SwayRider/protos.git
git clone git@github.com:SwayRider/swlib.git

# Generate protobuf code
cd protos && make

# Build services
go build ./...
```

### Mobile Development

```bash
# Requirements: Android Studio, JDK 17, Android SDK (compileSdk 36)

cd mobile
./gradlew assembleDebug
```

## Roadmap

| Milestone | Feature | Status |
|-----------|---------|--------|
| 001 | Foundation Complete | Done |
| 002 | Route Planning | In Progress |
| 003 | Turn-by-Turn Navigation | Planned |
| 004 | Advertisement Integration | Planned |
| 005 | Account Management | Planned |
| 006 | Polish & Play Store Release | Planned |

Current coverage: **Western Europe** (Belgium, Netherlands, Luxembourg, France, Germany, Iberian Peninsula)

---

<div align="center">

**SwayRider** — Ride further. Ride together.

</div>