# Deployment Guide

## Prerequisites
- SSH access to VPS
- Docker and docker-compose installed on server
- Git installed on server
- nginx installed (for reverse proxy)

## Deployment Steps

### 1. Clone Repository
```bash
cd /home/deploy/apps
git clone <your-repo-url> next-full-stack
cd next-full-stack
```

### 2. Set Environment Variables
Create `.env.production` file with required variables:
```bash
cat > .env.production << EOF
NODE_ENV=production
NEXTAUTH_SECRET=<generate-with-openssl-rand-hex-32>
NEXTAUTH_URL=https://your-domain.com
EOF
```

### 3. Build and Deploy with Docker Compose
```bash
# Set the port for this app
export APP_PORT=3001

# Build and start the container
docker-compose up -d --build

# Verify it's running
docker ps
docker logs next-full-stack
```

### 4. Configure Nginx Reverse Proxy
```bash
# Copy nginx config to sites-available
sudo cp nginx.conf /etc/nginx/sites-available/next-full-stack

# Edit the config with your domain
sudo nano /etc/nginx/sites-available/next-full-stack

# Create symlink to sites-enabled
sudo ln -s /etc/nginx/sites-available/next-full-stack /etc/nginx/sites-enabled/

# Test nginx config
sudo nginx -t

# Reload nginx
sudo systemctl reload nginx
```

### 5. SSL Setup (Optional but Recommended)
```bash
# Install certbot
sudo apt install certbot python3-certbot-nginx

# Generate certificate
sudo certbot certonly --nginx -d your-domain.com -d www.your-domain.com

# Update nginx config to use SSL (uncomment SSL section in nginx.conf)
sudo certbot renew --dry-run  # Test auto-renewal
```

## Environment Variables Reference
- `APP_PORT`: Container port mapping (default: 3001)
- `NODE_ENV`: Set to 'production'
- `NEXTAUTH_SECRET`: Generate with: `openssl rand -hex 32`
- `NEXTAUTH_URL`: Your application URL

## Monitoring and Maintenance

### View Logs
```bash
docker logs -f next-full-stack
```

### Restart Container
```bash
docker-compose restart
```

### Update Application
```bash
git pull
docker-compose up -d --build
```

### Check Resource Usage
```bash
docker stats next-full-stack
```

## Useful Commands

### Remove Container
```bash
docker-compose down
docker volume prune  # Optional: remove unused volumes
```

### Backup Data
```bash
docker exec next-full-stack tar czf - /app/data > backup.tar.gz
```

### Multiple Apps Setup
Repeat the process for each app, changing:
- App name in `/home/deploy/apps/{app-name}`
- `APP_PORT` environment variable (3001, 3002, 4001, etc.)
- nginx server_name and upstream block

## Production Recommendations

✅ **Implemented:**
- Multi-stage Docker build for optimal image size
- Non-root user execution for security
- Resource limits (512MB memory, 0.5 CPU)
- Automatic restart policy
- Health checks
- Log rotation
- Environment variable support

✅ **Additional Considerations:**
- Set up automated backups
- Monitor logs and resource usage
- Plan capacity for multiple apps
- Use docker networks for app communication
- Keep docker images updated
