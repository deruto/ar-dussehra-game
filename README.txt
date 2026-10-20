MIDNIGHT IGNITION — RAVAN EDITION (AR)

PROJECT FILES
- midnight_ignition_aframe.html: A-Frame scene and game logic, now rendered
  as an AR overlay on the live device camera feed.
- ravan-model/model.glb: single, self-contained binary glTF. Its base-color,
  metallic-roughness, and normal textures are embedded directly inside the
  .glb (as bufferView-backed images), so no separate texture files are
  needed or referenced.

HOW THIS IS AN "AR SCENE"
This is camera-passthrough AR, not full 6DOF WebXR/ARCore tracking:
- A <video> element plays the device's rear camera (getUserMedia) as a
  fixed full-screen backdrop.
- The A-Frame <a-scene> canvas is rendered with renderer="alpha: true" and
  no scene.background, so it's transparent and the live camera shows
  through behind the 3D content.
- look-controls drives the view using the phone's gyroscope/compass
  (device orientation), so physically rotating the phone looks around —
  there's no position/surface tracking, so the model doesn't anchor to a
  real-world surface, it's a fixed point in virtual space you look toward.
- A small gate screen (tap "Enable Camera & Motion") requests camera
  access and, on iOS 13+, explicitly requests motion-sensor permission
  via DeviceOrientationEvent.requestPermission() (required there; Android
  generally doesn't need an explicit prompt for this).
- The model's material is fully metallic/fully rough, so it still gets a
  JS-generated reflective "environment" (via THREE.PMREMGenerator) assigned
  only to scene.environment — this does not affect the visible background.

RUNNING
- Must be served over HTTPS (or http://localhost during local dev) — browsers
  block getUserMedia and DeviceOrientationEvent.requestPermission on plain
  http://. Opening the file directly via file:// will not work.
- Keep midnight_ignition_aframe.html and the ravan-model folder (with
  model.glb inside it) together, at the same relative path.
- Best tested on an actual phone (Chrome on Android, Safari on iOS) so the
  rear camera and gyroscope are available; a laptop with a webcam will
  show the video feed but won't have meaningful look-around motion.
- Tap "Enable Camera & Motion" first; if camera or motion access is denied,
  the on-screen status explains why and lets you retry.
