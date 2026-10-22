# Phone Sensors

Single-file mobile web app (`index.html`, no dependencies) that live-displays every sensor a phone browser exposes. Each source gets a card with live values, units, sample rate (Hz), min/max, a rolling graph, and an "unavailable" state.

**Live:** https://kv1400x.github.io/phone-sensors/ · Companion to [Align at Home](https://kv1400x.github.io/align-at-home/)

## Sensors
- **DeviceMotionEvent**: acceleration x/y/z, accelerationIncludingGravity x/y/z (m/s²), rotationRate alpha/beta/gamma (°/s), interval (ms)
- **DeviceOrientationEvent**: alpha, beta, gamma, absolute, webkitCompassHeading, webkitCompassAccuracy; `deviceorientationabsolute` (Android)
- **Generic Sensor API**: Accelerometer, LinearAccelerationSensor, GravitySensor, Gyroscope, Magnetometer, AbsoluteOrientationSensor, RelativeOrientationSensor, AmbientLightSensor
- **Geolocation** watchPosition: lat, lon, altitude, accuracy, altitudeAccuracy, speed, heading
- **Screen**: orientation angle/type, devicePixelRatio, viewport/visualViewport, screen size
- **Battery API**, **Network Information**, **Touch** (points, force, radius, Pencil angles), **Visibility/focus**, **Wake Lock**
- **On tap only**: microphone level meter (dBFS), rear camera preview

Controls: Start sensors (calls `DeviceMotionEvent.requestPermission()` / `DeviceOrientationEvent.requestPermission()`), Pause, Record → export CSV/JSON (share sheet via `navigator.share`, falls back to download), dark-mode toggle.

## iOS Safari (iPhone) — what you get
**Exposed:** DeviceMotion (accel, accel+gravity, gyro rotationRate, ~60 Hz), DeviceOrientation (alpha/beta/gamma) + `webkitCompassHeading`/`webkitCompassAccuracy`, Geolocation, `screen.orientation` (iOS 16.4+), devicePixelRatio/viewport, `touch.force` (3D Touch devices; iPhone 12 has no 3D Touch so it reads 0 or 1), visibility, Wake Lock (iOS 16.4+), mic & camera after a tap.

**Not exposed:**
- Motion/orientation need a user tap + HTTPS + permission prompt (`requestPermission`)
- No Generic Sensor API at all (no Accelerometer/Gyroscope/Magnetometer/Orientation sensor classes)
- No raw magnetometer — only the derived compass heading
- No ambient light sensor
- No Battery API, no Network Information API
- No `deviceorientationabsolute` event; `absolute` is false
- No barometer/altimeter pressure, proximity, or step counter
- Sample rate is fixed (~60 Hz), not configurable

Android Chrome supports the Generic Sensor API (AmbientLightSensor behind a flag), Battery, Network Information and `deviceorientationabsolute`.
