# AlongGPX Docker Quick Start

## Which Compose File Do I Need?

```
START HERE
    │
    ├─ Just want to use AlongGPX? ────────────────────► docker-compose.ghcr.yml ✅
    │
    ├─ Modifying backend/frontend code? ──────────────► docker-compose.yml
    │
    └─ Developing frontend with hot reload? ──────────► docker-compose.dev.yml
```

## Setup Commands

### Option 1: GHCR Production (Recommended)

```bash
# 1. Create directory
mkdir -p alonggpx && cd alonggpx

# 2. Download compose file
curl -O https://raw.githubusercontent.com/rikmueller/alonggpx/main/deployment/docker-compose.ghcr.yml

# 3. Download .env template
curl -o .env https://raw.githubusercontent.com/rikmueller/alonggpx/main/deployment/.env.example

# 4. Pull and start
docker compose -f docker-compose.ghcr.yml pull
docker compose -f docker-compose.ghcr.yml up -d
```

Open: **http://localhost:3000**

---

### Option 2: Local Build from Source

```bash
# 1. Clone repository
git clone https://github.com/rikmueller/alonggpx.git
cd alonggpx/deployment

# 2. Copy .env template
cp .env.example .env

# 3. Build and start
docker compose up --build -d
```

Open: **http://localhost:3000**

---

### Option 3: Development Mode

```bash
# 1. Clone repository
git clone https://github.com/rikmueller/alonggpx.git
cd alonggpx/deployment

# 2. Copy .env template
cp .env.example .env

# 3. Start with hot reload
docker compose -f docker-compose.dev.yml up
```

Frontend with HMR: **http://localhost:3000**

---

## Common Commands

```bash
# View logs
docker compose logs -f

# Stop services
docker compose down

# Update images (GHCR only)
docker compose -f docker-compose.ghcr.yml pull
docker compose -f docker-compose.ghcr.yml up -d

# Rebuild (local builds)
docker compose up --build -d
```

---

## Full Documentation

See [README.md](README.md) for detailed comparison and migration guides.
