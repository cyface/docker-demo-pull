# Docker Demo Pull

Demo of pulling app image from the Docker Registry and running it with an nginx proxy.

Even if you don't have the docker-demo image locally, docker-compose will pull it down for you.

### Files:
- `docker-compose.yml` - Tells Docker compose to run the cyface/docker-demo image, and run nginx as a proxy in front.
- `nginx.conf` - configuration file for the nginx image - overlays the config file in the image when it runs.

### Commands:
- `docker-compose up` - starts django and nginx
- You can browse to http://localhost
- Try changing the port in `nginx.conf` to `50000`
  - Then stop docker-compose with `ctrl-c`
  - Start it again with `docker-compose up`
- Then browse to http://localhost:50000

### Notes on `docker-compose.yml`
- This overlays the file `/etc/nginx/nginx.conf` in the container with the local file `nginx.conf` in `read only` (ro) mode:
- `volumes:`
  - `./nginx.conf:/etc/nginx/nginx.conf:ro`

This ensures that django starts before nginx:
- `depends_on:`
  - `django`
    
### Notes on `nginx.conf`:
- The `events` section is required, but does nothing in this case.
- The `http` section tells nginx to listen on all IP addresses and host names on port 80.
- The proxy passes all traffic back to Django on port 8000 for everything under /.
- The hostname `django` is created automatically by `docker-compose` because that is what the service name in `docker-compose.yml` is.

