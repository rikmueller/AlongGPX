# Docker Compose Configurations

This directory contains Docker Compose configurations for different use cases.

## Quick Decision

| File | Use When | Best For |
|------|----------|----------|
| `docker-compose.yml` | Standard deployment | **Production & development** |
| `docker-compose.dev.yml` | Frontend development | Hot reload, debugging |

## File Details

### docker-compose.yml

**Purpose:** Standard deployment with local builds from source code

**Advantages:**
- ✅ Full control over build
- ✅ Can modify and rebuild code
- ✅ Works offline (after initial build)

**Usage:**
```bash
docker compose up --build -d
```

**When to use:**
- You're deploying AlongGPX
- You're modifying backend or frontend code
- You need a custom build

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

## Common Configuration

Both compose files share:
- Same port mappings (backend: 5000, frontend: 3000)
- Same volume structure (data/input, data/output)
- Same environment variable handling (.env file)
- Health checks for both services

---

## Questions?

- **"Which compose file should I use?"** → Start with `docker-compose.yml`
- **"How do I customize presets?"** → See [../docs/quickstart-docker.md](../docs/quickstart-docker.md)
- **"Can I switch between them?"** → Yes, just bring down one and start another
- **"Do I need to rebuild images?"** → Yes, use `docker compose up --build -d`
