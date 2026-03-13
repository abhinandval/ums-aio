# macOS Setup Guide for UMS-AIO

This guide is specifically for macOS users to set up the Universal Media Server stack.

## Prerequisites

### 1. Install Homebrew

Homebrew is the package manager for macOS. Install it by running:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

After installation, add Homebrew to your PATH:

```bash
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"
```

### 2. Install Colima and Git

Colima is a lightweight container runtime for macOS (Docker Desktop alternative):

```bash
brew install colima git
```

### 3. Start Colima

Colima needs to be running before you can use Docker commands:

```bash
colima start
```

To stop Colima:
```bash
colima stop
```

To check Colima status:
```bash
colima status
```

**Note:** Colima uses Lima under the hood and provides a Docker-compatible CLI. No need to install Docker Desktop!

## Installation

### 1. Clone the Repository

```bash
git clone git@github.com:abhinandval/ums-aio.git
cd ums-aio
```

Or using HTTPS:
```bash
git clone https://github.com/abhinandval/ums-aio.git
cd ums-aio
```

### 2. Start All Services

```bash
docker compose up -d
```

This will download and start:
- Jellyfin (Media Server)
- Sonarr (TV Show Management)
- Radarr (Movie Management)
- qBittorrent (Download Client)
- Prowlarr (Indexer Manager)

## Accessing Services

Once running, access each service in your browser:

| Service | URL | Description |
|---------|-----|-------------|
| **Jellyfin** | http://localhost:8096 | Media server for streaming |
| **Sonarr** | http://localhost:8989 | TV show automation |
| **Radarr** | http://localhost:7878 | Movie automation |
| **qBittorrent** | http://localhost:8080 | Torrent client |
| **Prowlarr** | http://localhost:9696 | Indexer management |

### Default Credentials

**qBittorrent:**
- Username: `admin`
- Password: Check logs: `docker compose logs qbittorrent | grep password`

**Other services:** No default credentials. Create user accounts on first run.

## Daily Usage Commands

```bash
# View all running services
docker compose ps

# View logs
docker compose logs -f

# View logs for specific service
docker compose logs -f jellyfin

# Stop all services
docker compose down

# Restart a specific service
docker compose restart sonarr

# Update all containers to latest images
docker compose pull && docker compose up -d
```

## Configuration

All configuration files are stored in the `./data/` directory:
- `./data/jellyfin/` - Jellyfin settings
- `./data/sonarr/` - Sonarr settings
- `./data/radarr/` - Radarr settings
- `./data/qbittorrent/` - qBittorrent settings
- `./data/prowlarr/` - Prowlarr settings

Media files go in `./media/`:
- `./media/movies/` - Movie library
- `./media/tv_shows/` - TV show library
- `./media/downloads/` - Download directory

## Troubleshooting

### Port Already in Use

If you get "port already allocated" errors, some services might already be running:

```bash
# Check what's using the port
lsof -i :8096

# Stop Colima and start fresh
colima stop
docker compose down
colima start
docker compose up -d
```

### Colima Won't Start

```bash
# Reset Colima (warning: removes all containers)
colima delete
colima start
```

### Permission Issues

Files are created with PUID/PGID 1000. If you have permission issues:

```bash
# Find your user/group ID
id -u  # usually 501 on macOS
id -g  # usually 20 on macOS

# Edit docker-compose.yml and update PUID/PGID values
```

## Project Structure

```
ums-aio/
├── docker-compose.yml      # Service definitions
├── data/                   # Configuration (gitignored)
│   ├── jellyfin/
│   ├── sonarr/
│   ├── radarr/
│   ├── qbittorrent/
│   └── prowlarr/
└── media/                  # Media storage (gitignored)
    ├── movies/
    ├── tv_shows/
    └── downloads/
```

## Next Steps

1. **Configure qBittorrent**: Set download directory to `/storage/downloads`
2. **Configure Sonarr**: Add root folder `/storage/tv_shows`
3. **Configure Radarr**: Add root folder `/storage/movies`
4. **Configure Prowlarr**: Add indexers and sync with Sonarr/Radarr
5. **Configure Jellyfin**: Add libraries for `/storage/movies` and `/storage/tv_shows`

Enjoy your media server! 🎬📺

## Contributing

This project follows [Conventional Commits](AGENTS.md). Please read the guidelines before contributing.
