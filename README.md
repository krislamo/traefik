# Traefik Deployment Repository

Traefik is excellent as a reverse proxy within Docker environments, boasting
features like automatic service discovery and automated HTTPS via Let's
Encrypt. While similar, the docker-compose files available here cater to
different use cases for deploying Traefik.

## Add services to Traefik
1. Define the external traefik network on the top-level networks key
```
networks:
  traefik:
    external: true
```

2. Attach your web container to Traefik's network via the service-level `networks` key
```
    networks:
      - traefik
```

3. Define routing for Traefik in labels, replacing "examplerouter" with something unique
```
    labels:
      traefik.http.routers.examplerouter.rule: Host(`www.example.org`)
      traefik.http.routers.examplerouter.entrypoints: websecure
      traefik.http.routers.examplerouter.tls: true
      traefik.http.services.examplerouter.loadbalancer.server.port: 80
      traefik.docker.network: traefik
```

## Variables
Here's a brief explanation of the variables used in the docker-compose files:

### Docker Settings
- `IMAGE`: The name of the Docker image (default: `traefik`).
- `VERSION`: The tag of the Docker image (default: `latest`).
- `NAME`: The name assigned to the created container (default: `traefik`).

### Traefik Settings
- `DASHBOARD`: Enable(=true) or disable(=false) the Traefik API dashboard (default: `false`).
- `DOMAIN`: The domain name where Traefik's dashboard is accessible (default: `traefik.local.krislamo.org`).
- `ENTRYPOINT`: The entry point for the dashboard (default: `local`).
- `EXPOSED_BY_DEFAULT`: Expose Docker containers by default without needing specific labels (default: `false`).

### Network Settings
- `NETWORK`: The Docker network to be used (default: `traefik`).
- `WEB_PORT`: Binding for the regular HTTP traffic (defaults vary).
- `WEBSECURE_PORT`: Binding for HTTPS traffic (default: `0.0.0.0:443:443`, only on HTTPS version).
- `LOCAL_PORT`: Binding for local HTTPS traffic (default: `127.0.0.1:8443:8443`).

### Other Settings
- `LOG_LEVEL`: Logging level (default: `ERROR`).
- `DEBUG`: Enable(=true) or turn off(=false) API debugging (default: `false`).


## License
This project is released under the 0BSD license, which allows for unrestricted
use, modification, and distribution.
