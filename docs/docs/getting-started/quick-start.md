---
sidebar_position: 2
title: Quick Start
description: Build a working camera app in 5 minutes with react-webcam-pro.
---

# Quick Start

This guide walks you through creating a working camera application from scratch.

## Minimal Example

The simplest possible camera component:

```tsx
import React, { useRef, useState } from 'react';
import { Camera, CameraRef } from 'react-webcam-pro';

const App = () => {
  const cameraRef = useRef<CameraRef>(null);
  const [photo, setPhoto] = useState<string | null>(null);

  const handleTakePhoto = () => {
    if (cameraRef.current) {
      const image = cameraRef.current.takePhoto();
      setPhoto(image as string);
    }
  };

  return (
    <div>
      <Camera ref={cameraRef} />
      <button onClick={handleTakePhoto}>📸 Take Photo</button>
      {photo && <img src={photo} alt="Captured" />}
    </div>
  );
};

export default App;
```

That's it — a fully functional camera in **under 20 lines of code**.

## What's happening here?

1. **`Camera` component** renders a live video feed from the user's camera.
2. **`useRef<CameraRef>`** gives you access to camera methods via a [React ref](https://react.dev/learn/manipulating-the-dom-with-refs).
3. **`takePhoto()`** captures the current frame and returns it as a base64 JPEG string.

## Full-Featured Example

Here's a more complete example with camera switching, torch control, and device selection:

```tsx
import React, { useRef, useState, useEffect } from 'react';
import { Camera, CameraRef } from 'react-webcam-pro';

const App = () => {
  const cameraRef = useRef<CameraRef>(null);
  const [photo, setPhoto] = useState<string | null>(null);
  const [numberOfCameras, setNumberOfCameras] = useState(0);
  const [devices, setDevices] = useState<MediaDeviceInfo[]>([]);
  const [activeDeviceId, setActiveDeviceId] = useState<string | undefined>();

  // Enumerate available video devices
  useEffect(() => {
    navigator.mediaDevices.enumerateDevices().then((allDevices) => {
      setDevices(allDevices.filter((d) => d.kind === 'videoinput'));
    });
  }, []);

  return (
    <div style={{ width: '100%', maxWidth: 640 }}>
      {/* Camera view */}
      <Camera
        ref={cameraRef}
        aspectRatio={16 / 9}
        facingMode="environment"
        numberOfCamerasCallback={setNumberOfCameras}
        videoSourceDeviceId={activeDeviceId}
        videoReadyCallback={() => console.log('Camera ready!')}
      />

      {/* Controls */}
      <div style={{ display: 'flex', gap: 8, marginTop: 8 }}>
        <button onClick={() => setPhoto(cameraRef.current?.takePhoto() as string)}>
          📸 Take Photo
        </button>

        <button
          onClick={() => cameraRef.current?.switchCamera()}
          disabled={numberOfCameras <= 1}
        >
          🔄 Switch Camera
        </button>

        <button onClick={() => cameraRef.current?.toggleTorch()}>
          🔦 Toggle Torch
        </button>
      </div>

      {/* Device picker */}
      <select onChange={(e) => setActiveDeviceId(e.target.value)}>
        {devices.map((device) => (
          <option key={device.deviceId} value={device.deviceId}>
            {device.label || `Camera ${device.deviceId.slice(0, 8)}`}
          </option>
        ))}
      </select>

      {/* Photo preview */}
      {photo && (
        <div style={{ marginTop: 16 }}>
          <img src={photo} alt="Captured" style={{ width: '100%' }} />
        </div>
      )}
    </div>
  );
};

export default App;
```

## Running Locally

Camera access requires **HTTPS**. During development on `localhost`:

- **Chrome**: Works on `http://localhost` automatically
- **Safari**: Requires HTTPS even for localhost
- **Firefox**: Works on `http://localhost` automatically

To start a local dev server with HTTPS (if using Create React App):

```bash
HTTPS=true npm start
```

## Next Steps

- Learn about all available [Props](/docs/api/props)
- Explore [Methods](/docs/api/methods) available via the camera ref
- See the [Guides](/docs/guides/switching-cameras) for specific use cases
