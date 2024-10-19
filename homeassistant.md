## Xiaomi LYWSD03MMC with Zigbee

### Setup
1. Flash the device to the zigbee firmware using [Telink Flasher](https://pvvx.github.io/ATC_MiThermometer/TelinkMiFlasher.html) from [pvvx](https://github.com/pvvx/ZigbeeTLc/?tab=readme-ov-file)
2. If zigbee2mqtt doesn't discover the device, reset it using a paperclip and shortcurcuit the two pins in the back. Hold the pins for about 10 seconds and upon reset, the display will show "---". I had the best success, if I put my phone with front-camera enabled on a table while holding the device, display down, over it.


### zigbee2mqtt

```yml
  z2m:
    image: ghcr.io/koenkk/zigbee2mqtt:1.40.2
    container_name: zigbee2mqtt  
    restart: unless-stopped
    depends_on:
        - mqtt
    devices:  
      - /dev/ttyACM0:/dev/ttyACM0  
    ports:  
      - "8124:8080"  
    volumes:
      - /run/udev:/run/udev:ro  
      - ./zigbee2mqtt:/app/data
    environment:
        - PUID=${PUID}
        - PGID=${PGID}
        - TZ=${TZ} 
    networks:
        - homeassistant
```
