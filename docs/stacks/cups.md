``` yaml
version: "3"
services:
    cups:
        image: anujdatar/cups
        container_name: cups
        restart: unless-stopped
        ports:
            - "631:631"
        devices:
            - /dev/bus/usb:/dev/bus/usb
        environment:
            - CUPSADMIN=admin
            - CUPSPASSWORD=<secure-password-here>
            - TZ="Asia/Kolkata"
        volumes:
            - <actual-path>/cups/config:/etc/cups

```