# Neo4j Service

A Docker Compose setup for running Neo4j database with Coolify compatibility.

## Overview

This repository contains a production-ready Docker Compose configuration for Neo4j 5, optimized for deployment with Coolify.

## Quick Start

### Local Development

1. Clone the repository:
   ```bash
   git clone https://github.com/xxtract/neo4JService.git
   cd neo4JService
   ```

2. Copy the environment template:
   ```bash
   cp .env.example .env
   ```

3. Update `.env` with your configuration (especially `NEO4J_PASSWORD`)

4. Start the service:
   ```bash
   docker-compose up -d
   ```

5. Access Neo4j Browser:
   - URL: http://localhost:7474
   - Username: `neo4j`
   - Password: (from `NEO4J_PASSWORD` in `.env`)

### Environment Variables

See `.env.example` for all available configuration options:

- **NEO4J_PASSWORD** - Database password (CHANGE THIS!)
- **NEO4J_BROWSER_PORT** - Neo4j Browser port (default: 7474)
- **NEO4J_BOLT_PORT** - Bolt driver port (default: 7687)
- **NEO4J_PAGECACHE_SIZE** - Page cache memory (default: 512M)
- **NEO4J_HEAP_SIZE** - Java heap size (default: 512M)
- **NEO4J_LOG_LEVEL** - Logging level (default: INFO)
- **DOCKER_CPUS** - CPU limit (default: 2)
- **DOCKER_MEMORY** - Memory limit (default: 1G)

## Coolify Deployment

This setup is optimized for Coolify deployment:

1. **Environment-based configuration** - All settings use environment variables
2. **Health checks** - Includes health check endpoint
3. **Named volumes** - Proper volume management for persistence
4. **Resource limits** - CPU and memory limits configured
5. **Production-ready** - Security and logging settings included

### Deploy to Coolify

1. Create a new Coolify service
2. Use Docker Compose as deployment method
3. Set environment variables from `.env` in Coolify settings
4. Configure persistent volumes for `neo4j_data`, `neo4j_logs`, `neo4j_plugins`, `neo4j_import`
5. Deploy

## Directory Structure

```
neo4JService/
├── docker-compose.yml          # Docker Compose configuration
├── .env.example                # Environment variable template
├── .gitignore                  # Git ignore rules
├── README.md                   # This file
├── data/                       # Neo4j database data (generated)
├── logs/                       # Neo4j logs (generated)
└── plugins/                    # Neo4j plugins directory
```

## Ports

- **7474** - Neo4j Browser (HTTP)
- **7687** - Bolt protocol (for applications)

## Security Notes

- **Change the default password** in `.env` before deployment
- Use strong passwords in production
- Restrict network access to database ports
- Keep Neo4j updated to latest patch version

## Troubleshooting

### Service won't start
- Check logs: `docker-compose logs neo4j`
- Verify ports aren't in use: `lsof -i :7474`
- Ensure sufficient disk space

### Connection refused
- Verify container is running: `docker-compose ps`
- Check port mappings: `docker-compose ps`
- Ensure firewall allows connections

### Out of memory
- Increase `NEO4J_HEAP_SIZE` in `.env`
- Adjust `DOCKER_MEMORY` if running in container
- Monitor with: `docker stats`

## Health Check

The service includes a health check endpoint. Check status with:
```bash
docker-compose ps
```

Health check runs every 30 seconds with a 10-second timeout.

## Integration

### n8n Integration
To connect from n8n or other applications, use:
- **Host**: `neo4j` (if same Docker network) or `localhost` (if local)
- **Port**: `7687` (Bolt)
- **Username**: `neo4j`
- **Password**: (from `NEO4J_PASSWORD`)

## Maintenance

### Backup Data
```bash
docker-compose exec neo4j neo4j-admin database dump neo4j --to-path=/data/backups
```

### Update Neo4j Version
1. Update `image: neo4j:X` in `docker-compose.yml`
2. Restart service: `docker-compose up -d`
3. Neo4j will auto-upgrade the database

### View Logs
```bash
docker-compose logs -f neo4j
```

## Support

For issues or questions:
- Check Neo4j documentation: https://neo4j.com/docs/
- GitHub Issues: https://github.com/xxtract/neo4JService/issues

## License

MIT License - See LICENSE file for details
