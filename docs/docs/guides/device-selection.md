---
sidebar_position: 5
title: Device Selection
description: How to let users pick a specific camera device.
---

# Device Selection

On devices with multiple cameras (or external webcams), you may want to let users choose which camera to use via a dropdown or other UI.

## Listing Available Devices

Use the browser's `navigator.mediaDevices.enumerateDevices()` API to get a list of video input devices:

```tsx
import { useEffect, useState } from 'react';

const [devices, setDevices] = useState<MediaDeviceInfo[]>([]);

useEffect(() => {
  navigator.mediaDevices.enumerateDevices().then((allDevices) => {
    const videoDevices = allDevices.filter((d) => d.kind === 'videoinput');
    setDevices(videoDevices);
  });
}, []);
```

:::note
The `label` property of `MediaDeviceInfo` is empty until the user grants camera permission. After the first successful camera access, device labels become available.
:::

## Using `videoSourceDeviceId`

Pass the selected device's ID to the `videoSourceDeviceId` prop:

```tsx
const [activeDeviceId, setActiveDeviceId] = useState<string | undefined>();

<Camera
  ref={cameraRef}
  videoSourceDeviceId={activeDeviceId}
  facingMode="environment"
/>

<select onChange={(e) => setActiveDeviceId(e.target.value)}>
  <option value="">Auto</option>
  {devices.map((device) => (
    <option key={device.deviceId} value={device.deviceId}>
      {device.label || `Camera ${device.deviceId.slice(0, 8)}...`}
    </option>
  ))}
</select>
```

## Priority Behavior

When `videoSourceDeviceId` is provided:

1. **It takes priority** over the `facingMode` auto-detection logic.
2. The constraint uses `{ deviceId: { exact: id } }` to ensure the exact device is used.
3. When `videoSourceDeviceId` changes, the camera stream automatically reinitializes.

When `videoSourceDeviceId` is **not** provided (or `undefined`):

1. The component uses `facingMode` to select the camera.
2. On devices with multiple rear cameras, it intelligently selects the best one by checking device capabilities.

:::tip Fixed in v1.0.0
In the original `react-camera-pro`, the auto-detection logic for environment cameras could **override** the explicitly set `videoSourceDeviceId`. This has been fixed — device ID selection always takes priority when provided.
:::

## Complete Example

```tsx
import { useRef, useState, useEffect } from 'react';
import { Camera, CameraRef } from 'react-camera-web';

const App = () => {
  const cameraRef = useRef<CameraRef>(null);
  const [devices, setDevices] = useState<MediaDeviceInfo[]>([]);
  const [activeDeviceId, setActiveDeviceId] = useState<string>();

  useEffect(() => {
    navigator.mediaDevices.enumerateDevices().then((allDevices) => {
      setDevices(allDevices.filter((d) => d.kind === 'videoinput'));
    });
  }, []);

  return (
    <div>
      <Camera
        ref={cameraRef}
        videoSourceDeviceId={activeDeviceId}
        facingMode="environment"
      />

      <label>
        Camera:
        <select onChange={(e) => setActiveDeviceId(e.target.value || undefined)}>
          <option value="">Auto (use facingMode)</option>
          {devices.map((device) => (
            <option key={device.deviceId} value={device.deviceId}>
              {device.label || `Camera ${device.deviceId.slice(0, 8)}...`}
            </option>
          ))}
        </select>
      </label>
    </div>
  );
};
```
