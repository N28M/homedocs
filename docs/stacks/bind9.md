``` yaml
version: "3"
services:
  bind9:
    container_name: bind9
    image: ubuntu/bind9:latest
    environment:
      - BIND9_USER=root
      - TZ=Asia/Kolkata
    ports:
      - "53:53/tcp"
      - "53:53/udp"
    volumes:
      - <actual-path>/bind9/config:/etc/bind
      - <actual-path>/bind9/cache:/var/cache/bind
      - <actual-path>/bind9/records:/var/lib/bind
    restart: unless-stopped
```