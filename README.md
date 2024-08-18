# hoermann-shelly

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
