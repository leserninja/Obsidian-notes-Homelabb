
Homer är en simple web dashboard för att centeralisera antigen health checks eller bara länka alla dina olika web services eller vad du har.

konfigurera dashboarden, gå via "/opt/homer/assets/config.yml"  i lxc, i docker "/www/assets/"

Enkelt installerat via lxc på communicty scripts eller via docker compose som ser ut så här nedan: 
#### Docker compose:
```
services:
  homer:
    image: b4bz/homer
    container_name: homer
    volumes:
      - /path/to/config/dir:/www/assets # Make sure your local config directory exists
    ports:
      - 8080:8080
    user: 1000:1000 # default
    environment:
      - INIT_ASSETS=1 # default, requires the config directory to be writable for the container user (see user option)
    restart: unless-stopped
```

#### Dockerfile:

```
docker run -d \
  --name homer \
  -p 8080:8080 \
  --mount type=bind,source="/path/to/config/dir",target=/www/assets \
  --restart=unless-stopped \
  b4bz/homer:latest
```

