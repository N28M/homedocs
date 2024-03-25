``` yaml
---
version: "2.1"
services:
  plex-meta-manager:
    image: meisnate12/plex-meta-manager:latest
    container_name: plex-meta-manager
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Asia/Kolkata
      - PMM_CONFIG=/config/config.yml #optional
      - PMM_TIME=01:00 #optional
      - PMM_RUN=False #optional
      - PMM_TEST=False #optional
      - PMM_NO_MISSING=False #optional
    volumes:
      - <actual-path>/plex-meta-manager/config:/config
    restart: always
```