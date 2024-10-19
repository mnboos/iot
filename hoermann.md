# Öffnungsgrad von Hörmann Garangentor mit Shelly Plus Uni via MQTT senden

## Shelly Script
```javascript
function bleCallback(ev, res) {
    if (ev === BLE.Scanner.SCAN_RESULT && res.addr === "d3:3a:23:b4:f5:c9") {
        let sixthByte = res.scanRsp.charCodeAt(5);
        let openPercent = Math.round(sixthByte / 2);
        console.log("percentage: ", openPercent);
        MQTT.publish("shelly/hoermann/garagentor/offen", JSON.stringify(openPercent), 0, false);
    }
}

if (!BLE.Scanner.isRunning()) {
  BLE.Scanner.Start({
    duration_ms: -1,
    active: true,
    interval_ms: 100,
    window_ms: 30,
  });
}

BLE.Scanner.Subscribe(bleCallback);
```

## Home Assistant MQTT Sensor
```yml
mqtt:
    sensor:
        - name: 'Garage Öffnungsgrad'
          object_id: 'shelly_hoermann'
          availability_mode: latest
          state_topic: 'shelly/hoermann/garagentor/offen'
          unit_of_measurement: '%'
          value_template: '{{ value_json }}'
```
