# Docker Compose Files Comparison

This directory contains three Docker Compose configurations for different use cases. Choose the one that fits your needs.

## Quick Decision Table

| File | Use When | Build Required | Images From | Best For |
|------|----------|----------------|-------------|----------|
| **`docker-compose.ghcr.yml`** ✅ | **Production deployment** | ❌ No | GHCR pre-built | **Recommended for most users** |
| `docker-compose.yml` | Modifying source code | ✅ Yes | Local build | Development/customization |
| `docker-compose.dev.yml` | Frontend development | ✅ Yes | Local build | Hot reload, debugging |

## File Details

### docker-compose.ghcr.yml (Recommended)

**Purpose:** Production deployment using pre-built images from GitHub Container Registry

**Advantages:**
- ✅ No build step - just pull and run
- ✅ Fastest startup time
- ✅ Always gets latest stable release
- ✅ Minimal local dependencies

**Usage:**
```bash
docker compose -f docker-compose.ghcr.yml pull
docker compose -f docker-compose.ghcr.yml up -d
```

**When to use:**
- You just want to use AlongGPX
- Production deployments
- You're not modifying source code

---

### docker-compose.yml

**Purpose:** Local builds from source code in parent directory

**Header:** `LOCAL BUILD VERSION`

**Advantages:**
- ✅ Full control over build
- ✅ Can modify source code
- ✅ See your changes immediately

**Usage:**
```bash
docker compose up --build
```

**When to use:**
- You're modifying backend or frontend code
- You need a custom build
- You're developing new features

---

### docker-compose.dev.yml

**Purpose:** Development mode with hot reload for frontend

**Advantages:**
- ✅ Frontend hot reload (changes reflect instantly)
- ✅ Volume-mounted source code
- ✅ Better for iterative frontend development

**Usage:**
```bash
docker compose -f docker-compose.dev.yml up
```

**When to use:**
- You're actively developing frontend components
- You need instant feedback on UI changes
- You're debugging frontend issues

---

## Migration Examples

### From Local Build to GHCR

```bash
# Stop local build containers
docker compose down

# Switch to GHCR
docker compose -f docker-compose.ghcr.yml pull
docker compose -f docker-compose.ghcr.yml up -d
```

### From GHCR to Local Build

```bash
# Stop GHCR containers
docker compose -f docker-compose.ghcr.yml down

# Build and run locally
docker compose up --build -d
```

---

## Common Configuration

All three compose files share:
- Same port mappings (backend: 5000, frontend: 3000)
- Same volume structure (data/input, data/output)
- Same environment variable handling (.env file)
- Health checks for both services

---

## Questions?

- **"Which compose file should I use?"** → Start with `docker-compose.ghcr.yml`
- **"How do I customize presets?"** → See [../docs/quickstart-docker.md](../docs/quickstart-docker.md)
- **"Can I switch between them?"** → Yes, just bring down one and start another
- **"Do I need to rebuild images?"** → Only for `docker-compose.yml` and `docker-compose.dev.yml`
