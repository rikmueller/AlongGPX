# AlongGPX - Docker Quickstart (Production)

Deploy AlongGPX using Docker Compose with pre-built images from GitHub Container Registry (GHCR).

## 🚀 Quick Start

### Prerequisites
- Docker 20.10+
- Docker Compose 2.0+ (or docker compose plugin)
- 2GB free disk space
- Internet connection (for pulling images)

### Option A: Clone Repository (Recommended)

Get the complete setup with example files:

```bash
# Clone repository
git clone https://github.com/rikmueller/alonggpx.git
cd alonggpx/deployment

# Copy and configure environment file
cp .env.example .env
nano .env  # Edit configuration (see Configuration section below)

# Pull latest images
docker compose pull

# Start services
docker compose up -d
```

Open your browser to **http://localhost:3000**

### Option B: Manual Setup

Create your own deployment without cloning:

#### 1. Create directory structure

```bash
mkdir -p alonggpx/deployment/{data/{input,output}}
cd alonggpx/deployment
```

#### 2. Download configuration files

```bash
# Download docker-compose.yml
curl -o docker-compose.yml \
  https://raw.githubusercontent.com/rikmueller/alonggpx/main/deployment/docker-compose.yml

# Download .env.example
curl -o .env.example \
  https://raw.githubusercontent.com/rikmueller/alonggpx/main/deployment/.env.example

# Download presets file
curl -o ../data/presets.yaml \
  https://raw.githubusercontent.com/rikmueller/alonggpx/main/data/presets.yaml

# Copy and configure environment file
cp .env.example .env
nano .env  # Edit your configuration
```

#### 3. Start services

```bash
docker compose pull
docker compose up -d
```

Open your browser to **http://localhost:3000**

---

## 📋 How to Use

1. **Upload GPX**: Drag and drop your GPX track file
2. **Configure**: Set search radius and choose filter presets
3. **Generate**: Click "Generate Results" and monitor progress
4. **Download**: Get Excel spreadsheet or interactive HTML map

---

## ⚙️ Configuration with .env File

The `.env` file in the `deployment/` directory controls application behavior. This file is loaded by Docker Compose and provides default values.

### Understanding .env Structure

The `.env` file uses `KEY=VALUE` format (no spaces around `=`):

```bash
# Comments start with #
ALONGGPX_RADIUS_KM=5        # Search radius in kilometers
ALONGGPX_PROJECT_NAME=MyTrip  # Default project name
```

### Configuration Precedence

Settings are applied in this order (highest priority first):

1. **Web UI form inputs** - Values entered when generating results
2. **`.env` file** - Your custom defaults in `deployment/.env`
3. **Built-in defaults** - Hardcoded fallbacks in the application

**Example**: If you set `ALONGGPX_RADIUS_KM=10` in `.env`, all processing will use 10km radius by default. However, if a user enters "5" in the web UI, that specific job will use 5km instead.

### Essential Settings

Edit your `.env` file:

```bash
# Project Settings
ALONGGPX_PROJECT_NAME=MyProject    # Default name for output files
ALONGGPX_TIMEZONE=UTC              # Timezone for timestamps (e.g., Europe/Berlin)

# Search Parameters
ALONGGPX_RADIUS_KM=5               # Search radius around track (km)

# Optional: Default Presets (semicolon-separated)
ALONGGPX_PRESETS=camp_sites_tent;drinking_water

# Optional: Default Filters (semicolon-separated)
ALONGGPX_SEARCH_INCLUDE=amenity=shelter
ALONGGPX_SEARCH_EXCLUDE=
```

### Advanced Settings

These settings are pre-configured in `docker-compose.yml` but can be overridden in `.env`:

```bash
# Overpass API Settings
ALONGGPX_BATCH_KM=50                    # km of track per API call (higher = fewer calls)
ALONGGPX_OVERPASS_RETRIES=3            # Retry attempts for failed queries

# Cleanup Settings
ALONGGPX_CLEANUP_INTERVAL_SECONDS=600   # How often to run cleanup (10 minutes)
ALONGGPX_JOB_TTL_SECONDS=21600         # Keep completed jobs for 6 hours
ALONGGPX_OUTPUT_RETENTION_DAYS=10       # Delete output files after 10 days

# Map Styling
ALONGGPX_TRACK_COLOR=blue
ALONGGPX_DEFAULT_MARKER_COLOR=gray
```

### Applying Configuration Changes

After editing `.env`, restart the services:

```bash
cd deployment
docker compose down
docker compose up -d
```

---

## 🔄 Common Operations

### View logs

```bash
cd deployment

# Follow logs in real-time
docker compose logs -f

# View last 100 lines
docker compose logs --tail=100

# View only backend logs
docker compose logs -f app
```

### Stop services

```bash
cd deployment
docker compose down
```

### Restart services

```bash
cd deployment
docker compose restart
```

### Update to latest version

```bash
cd deployment
docker compose pull
docker compose up -d

```

### Check container status

```bash
cd deployment
docker compose ps
```

---

## 📂 Directory Structure

After setup, your directory should look like this:

```
alongGPX/
├── deployment/
│   ├── docker-compose.yml       # Service definitions
│   ├── .env                     # Your configuration (edit this!)
│   ├── .env.example             # Example configuration
│   ├── Dockerfile               # Backend image build instructions
│   ├── Dockerfile.nginx         # Frontend image build instructions
│   └── nginx.conf               # Nginx reverse proxy config
└── data/
    ├── presets.yaml             # Filter presets (mounted read-only)
    ├── input/                   # Place your GPX files here
    │   └── track.gpx
    └── output/                  # Generated files appear here
        ├── *.xlsx               # Excel spreadsheets
        └── *.html               # Interactive maps
```

---

## 🔧 Environment Variables Reference

Complete reference of all available environment variables:

| Variable | Default | `.env` Usage | Description |
|----------|---------|--------------|-------------|
| `ALONGGPX_PROJECT_NAME` | `AlongGPX` | ✅ Edit in `.env` | Default project name for output files |
| `ALONGGPX_RADIUS_KM` | `5` | ✅ Edit in `.env` | Search radius around track (km) |
| `ALONGGPX_TIMEZONE` | `UTC` | ✅ Edit in `.env` | Timezone for timestamps (e.g., Europe/Berlin) |
| `ALONGGPX_PRESETS` | _(empty)_ | ✅ Edit in `.env` | Default presets (semicolon-separated) |
| `ALONGGPX_SEARCH_INCLUDE` | _(empty)_ | ✅ Edit in `.env` | Default include filters (semicolon-separated) |
| `ALONGGPX_SEARCH_EXCLUDE` | _(empty)_ | ✅ Edit in `.env` | Default exclude filters (semicolon-separated) |
| `ALONGGPX_BATCH_KM` | `50` | ⚙️ Advanced | km of track per Overpass API call |
| `ALONGGPX_OVERPASS_RETRIES` | `3` | ⚙️ Advanced | Retry attempts for failed queries |
| `ALONGGPX_CLEANUP_INTERVAL_SECONDS` | `600` | ⚙️ Advanced | Cleanup job interval (10 minutes) |
| `ALONGGPX_JOB_TTL_SECONDS` | `21600` | ⚙️ Advanced | Keep completed jobs for 6 hours |
| `ALONGGPX_OUTPUT_RETENTION_DAYS` | `10` | ⚙️ Advanced | Delete old output files after N days |
| `ALONGGPX_TRACK_COLOR` | `blue` | ⚙️ Advanced | Track polyline color on maps |
| `ALONGGPX_DEFAULT_MARKER_COLOR` | `gray` | ⚙️ Advanced | Fallback marker color |

**Legend:**
- ✅ **Edit in `.env`** - Common settings you'll likely customize
- ⚙️ **Advanced** - Pre-configured in `docker-compose.yml`, override only if needed

---

## 🗂️ Volume Mounts Explained

Docker Compose automatically mounts these directories from your repository:

```yaml
volumes:
  - ../data/input:/app/data/input:ro        # GPX files (read-only)
  - ../data/output:/app/data/output:rw      # Results (read-write)
  - ../data/presets.yaml:/app/data/presets.yaml:ro  # Filter presets (read-only)
```

---

## 🌐 Production Deployment

### Behind Reverse Proxy (Nginx)

```nginx
upstream alonggpx {
    server localhost:3000;
}

server {
    listen 443 ssl http2;
    server_name gpx.example.com;
    
    ssl_certificate /etc/ssl/certs/gpx.example.com.crt;
    ssl_certificate_key /etc/ssl/private/gpx.example.com.key;
    
    # Security headers
    add_header X-Frame-Options "SAMEORIGIN" always;
    add_header X-XSS-Protection "1; mode=block" always;
    add_header X-Content-Type-Options "nosniff" always;
    
    # Max upload size
    client_max_body_size 50M;
    
    location / {
        proxy_pass http://alonggpx;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

### With Traefik

```yaml
services:
  alonggpx:
    image: ghcr.io/rikmueller/alonggpx:latest
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.alonggpx.rule=Host(`gpx.example.com`)"
      - "traefik.http.routers.alonggpx.entrypoints=websecure"
      - "traefik.http.routers.alonggpx.tls.certresolver=letsencrypt"
      - "traefik.http.services.alonggpx.loadbalancer.server.port=80"
    networks:
      - traefik
```

### Resource Limits

```yaml
services:
  alonggpx:
    # ... other config
    deploy:
      resources:
        limits:
          cpus: '2'
          memory: 2G
        reservations:
          cpus: '0.5'
          memory: 512M
```

---

## 🔍 Health Checks & Monitoring

### Check Container Health

```bash
docker ps
# Look for "healthy" status
```

### Manual Health Check

```bash
curl http://localhost:3000/health
# Should return: {"status": "healthy", "service": "AlongGPX"}
```

### View Logs

```bash
# Follow logs
docker logs -f alonggpx

# Last 100 lines
docker logs --tail=100 alonggpx

# With timestamps
docker logs --timestamps alonggpx
```

---

## 🐛 Troubleshooting

### Container won't start

```bash
# Check logs
cd deployment
docker compose logs app

# Check if port is already in use
netstat -tulpn | grep 3000

# Try different port (edit docker-compose.yml)
# Change: "3000:80" to "8080:80"
docker compose down
docker compose up -d
```

### "Permission denied" on volumes

```bash
# Fix output directory permissions
sudo chmod 755 data/output
sudo chown -R $(id -u):$(id -g) data/output
```

### Overpass API timeouts

- Increase `ALONGGPX_BATCH_KM` to reduce number of queries
- Check Overpass API status: https://overpass-api.de/api/status
- Wait a few minutes and try again

### No results found

- Verify filter syntax: `key=value` format
- Test filters at https://overpass-turbo.eu/
- Check that POIs exist in your area on OpenStreetMap

### Job stuck in "processing"

```bash
# Check backend logs for errors
cd deployment
docker compose logs -f app | grep ERROR
```

- Large GPX files may take several minutes
- Very dense areas may timeout - increase `ALONGGPX_BATCH_KM` in `.env`
- Try smaller radius or use fewer filters

---

## 🔄 Updating to Latest Version

```bash
cd deployment

# Pull latest images
docker compose pull

# Restart with new images
docker compose up -d

# Clean up old images
docker image prune -a
```

---

## 📊 Data Persistence

### Backup Output Files

```bash
# Create backup
tar -czf alonggpx-backup-$(date +%Y%m%d).tar.gz data/output/

# Restore backup
tar -xzf alonggpx-backup-20260130.tar.gz
```

### Automatic Cleanup

AlongGPX automatically cleans up:
- Old job entries (default: 6 hours)
- Temporary files (default: 1 hour)
- Output files (default: 10 days)

Configure via environment variables (see Configuration section).

---

## 🏗️ Advanced: Building from Source

If you need to modify the source code or build your own images:

```bash
# Clone repository
git clone https://github.com/rikmueller/alonggpx.git
cd alonggpx/deployment

# Edit .env configuration
cp .env.example .env
nano .env

# Build images from source (instead of pulling from GHCR)
docker compose build

# Start with custom build
docker compose up -d

# View build logs
docker compose logs -f
```

For development workflows with hot reload, see [docs/quickstart-dev.md](quickstart-dev.md).

---

## 📚 Next Steps

- **Add custom presets** - Edit `data/presets.yaml`
- **Integrate with automation** - See REST API documentation
- **Set up monitoring** - Add Prometheus/Grafana
- **Scale horizontally** - Run multiple instances behind load balancer

For development and customization, see [docs/quickstart-dev.md](quickstart-dev.md).

