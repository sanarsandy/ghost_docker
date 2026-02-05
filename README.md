# Ghost Docker Template

Production-ready Docker template for self-hosted Ghost blog with Nginx reverse proxy.

## Features

- ✅ **Multi-instance support** - Deploy multiple Ghost blogs on same server
- ✅ **Fully configurable** - Just edit `.env` file
- ✅ **Auto-generated Nginx config** - Run `setup.sh`
- ✅ **MySQL database** - With healthcheck
- ✅ **Security hardened** - Rate limiting, security headers
- ✅ **SSL ready** - Let's Encrypt compatible

## Quick Start

### 1. Clone & Configure

```bash
git clone https://github.com/sanarsandy/ghost_docker.git my-blog
cd my-blog
cp .env.example .env
nano .env  # Edit your configuration
```

### 2. Configure `.env`

```bash
# Unique prefix for this instance
CONTAINER_PREFIX=myblog

# Your domain
DOMAIN=myblog.example.com
GHOST_URL=https://myblog.example.com

# Unique port (2368, 2369, 2370, etc.)
GHOST_PORT=2368

# Database passwords
DB_ROOT_PASSWORD=secure_root_password
DB_GHOST_PASSWORD=secure_ghost_password
```

### 3. Generate Nginx Config

```bash
chmod +x setup.sh
./setup.sh
```

### 4. Deploy to Server

```bash
# Copy Nginx config
sudo cp nginx/${CONTAINER_PREFIX}.conf /etc/nginx/sites-available/
sudo ln -sf /etc/nginx/sites-available/${CONTAINER_PREFIX} /etc/nginx/sites-enabled/

# Copy proxy-params snippet (first time only)
sudo cp nginx/proxy-params.conf /etc/nginx/snippets/

# Get SSL certificate
sudo certbot --nginx -d your-domain.com

# Test & reload Nginx
sudo nginx -t && sudo systemctl reload nginx

# Start Ghost
docker-compose up -d
```

## Multiple Instances Example

| Instance | CONTAINER_PREFIX | GHOST_PORT | DOMAIN |
|----------|-----------------|------------|--------|
| Blog 1   | ghost           | 2368       | ghost.edukra.id |
| Blog 2   | indra           | 2369       | indra.edukra.id |
| Blog 3   | company         | 2370       | blog.company.com |

Each instance needs its own folder with separate `.env` configuration.

## File Structure

```
ghost_docker/
├── docker-compose.yml    # Docker orchestration
├── Dockerfile            # Ghost image
├── .env.example          # Configuration template
├── .gitignore            # Security exclusions
├── setup.sh              # Nginx config generator
└── nginx/
    ├── proxy-params.conf # Proxy settings
    └── [generated].conf  # Auto-generated per instance
```

## License

MIT
