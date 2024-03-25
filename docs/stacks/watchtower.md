``` yaml
version: "3"
services:
  watchtower:
    image: containrrr/watchtower
    container_name: watchtower
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Asia/Kolkata
      - WATCHTOWER_CLEANUP=true
      - WATCHTOWER_INCLUDE_STOPPED=true
      - WATCHTOWER_REVIVE_STOPPED=false
      - WATCHTOWER_SCHEDULE=0 0 6 * * *
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
    network_mode: host
    restart: unless-stopped
```