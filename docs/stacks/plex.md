``` yaml
---
version: "2.1"
services:
  plex:
    image: lscr.io/linuxserver/plex:latest
    container_name: plex
    network_mode: host
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Asia/Kolkata
      - VERSION=docker
    volumes:
      - <actual-path>/plex/config:/config
      - <media-path>/Data/Media/TV:/tv
      - <media-path>/Data/Media/Movies:/movies
    restart: unless-stopped

```