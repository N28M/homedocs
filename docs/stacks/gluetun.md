``` yaml
version: "3"
services:
  gluetun:
    image: qmcgaw/gluetun:v3.37.0
    hostname: gluetun
    container_name: gluetun
    cap_add:
      - NET_ADMIN
    devices:
      - /dev/net/tun:/dev/net/tun
    volumes:
      - <actual-path>/gluetun/config:/gluetun
    environment:
      #Airvpn OpenVPN
      - VPN_SERVICE_PROVIDER=airvpn
      - SERVER_COUNTRIES=Netherlands
      - FIREWALL_VPN_INPUT_PORTS=<airvpn-forwarded-port>
    ports:
      - <airvpn-forwarded-port>:<airvpn-forwarded-port>/tcp #Airvpn forwarded port
      - <airvpn-forwarded-port>:<airvpn-forwarded-port>/udp #Airvpn forwarded port
      - 8585:8585/tcp #qbittorrent
      - 5055:5055/tcp #overseer
      - 7878:7878/tcp #radarr
      - 7777:7777/tcp #radarr4k
      - 9696:9696/tcp #prowlar
      - 9117:9117/tcp #jackett
      - 8191:8191/tcp #flaresolverr
      - 8989:8989/tcp #sonarr
      - 6767:6767/tcp #bazarr (radarr and sonarr)
      - 6868:6868/tcp #bazarr (radarr4k)
      - 3001:3001/tcp #chrome
    restart: always

  chromium:
    image: lscr.io/linuxserver/chromium:latest
    container_name: chromium
    network_mode: "service:gluetun"
    security_opt:
      - seccomp:unconfined #optional
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Asia/Kolkata
    volumes:
      - <actual-path>/chromium/config:/config
    shm_size: "1gb"
    restart: always

  qbittorrent:
    image: lscr.io/linuxserver/qbittorrent:latest
    container_name: qbittorrent
    network_mode: "service:gluetun"
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Asia/Kolkata
      - WEBUI_PORT=8585
    volumes:
      - <actual-path>/qbittorrent/config:/config
      - <download-location-path>/Download/Torrents:/downloads
      #- <actual-path>/qbittorrent/vuetorrent:/vuetorrent
    restart: always

  overseerr:
    image: linuxserver/overseerr:latest
    container_name: overseerr
    network_mode: "service:gluetun"

    environment:
      - TZ=Asia/Kolkata # Specify a timezone to use
      - PUID=1000 # User ID to run as
      - PGID=1000 # Group ID to run as
    volumes:
      - <actual-path>/overseer/config:/config # Contains all relevant configuration files.
    restart: always

  prowlarr:
    image: lscr.io/linuxserver/prowlarr:latest
    container_name: prowlarr
    network_mode: "service:gluetun"
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Asia/Kolkata
    volumes:
      - <actual-path>/prowlarr/config:/config
    restart: always

  flaresolverr:
    image: ghcr.io/flaresolverr/flaresolverr:latest
    container_name: flaresolverr
    network_mode: "service:gluetun"
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Asia/Kolkata
    volumes:
      - <actual-path>/flaresolverr/config:/config:/config
    restart: always

  radarr:
    image: lscr.io/linuxserver/radarr:latest
    container_name: radarr
    network_mode: "service:gluetun"
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Asia/Kolkata
    volumes:
      - <actual-path>/radarr/config:/config
      - /srv/mergerfs/Merge/Data/Media/Movies/1080p:/movies #optional
      - <download-location-path>/Download/Torrents:/downloads #optional
    restart: always

  radarr4k:
    image: lscr.io/linuxserver/radarr:latest
    container_name: radarr4k
    network_mode: "service:gluetun"
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Asia/Kolkata
    volumes:
      - <actual-path>/radarr4k/config:/config
      - /srv/mergerfs/Merge/Data/Media/Movies/2160p:/movies #optional
      - <download-location-path>/Download/Torrents:/downloads #optional
    restart: always

  sonarr:
    image: lscr.io/linuxserver/sonarr:latest
    container_name: sonarr
    network_mode: "service:gluetun"
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Asia/Kolkata
    volumes:
      - <actual-path>/sonarr/config:/config
      - /srv/mergerfs/Merge/Data/Media/TV:/tv #optional
      - <download-location-path>/Download/Torrents:/downloads #optional
    restart: always

  bazarr:
    image: lscr.io/linuxserver/bazarr:latest
    container_name: bazarr
    network_mode: "service:gluetun"
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Asia/Kolkata
    volumes:
      - <actual-path>/bazarr/config:/config
      - /srv/mergerfs/Merge/Data/Media/Movies/1080p:/movies #optional
      - /srv/mergerfs/Merge/Data/Media/TV:/tv #optional
    restart: always

  bazarr4k:
    image: lscr.io/linuxserver/bazarr:latest
    container_name: bazarr4k
    network_mode: "service:gluetun"
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Asia/Kolkata
    volumes:
      - <actual-path>/bazarr4k/config:/config
      - /srv/mergerfs/Merge/Data/Media/Movies/2160p:/movies #optional
    restart: always
```