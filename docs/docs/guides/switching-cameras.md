---
sidebar_position: 1
title: Switching Cameras
description: How to switch between front and back cameras.
---

# Switching Cameras

Most mobile devices have at least two cameras — front (user) and back (environment). `react-webcam-pro` makes it easy to switch between them.

## Basic Camera Switching

```tsx
import { useRef, useState } from 'react';
import { Camera, CameraRef } from 'react-webcam-pro';

const App = () => {
  const cameraRef = useRef<CameraRef>(null);
  const [numberOfCameras, setNumberOfCameras] = useState(0);

  return (
    <div>
      <Camera
        ref={cameraRef}
        numberOfCamerasCallback={setNumberOfCameras}
      />

      <button
        onClick={() => cameraRef.current?.switchCamera()}
        disabled={numberOfCameras <= 1}
      >
        🔄 Switch Camera
      </button>
    </div>
  );
};
```

## How It Works

1. The `numberOfCamerasCallback` prop tells you how many cameras are available.
2. `switchCamera()` toggles between `'user'` and `'environment'` facing modes.
3. The method returns the **new** facing mode after switching.

## Setting the Default Camera

Use the `facingMode` prop to set which camera starts first:

```tsx
// Start with the back camera
<Camera facingMode="environment" />

// Start with the front camera (default)
<Camera facingMode="user" />
```

## Tracking the Current Mode

```tsx
const [currentMode, setCurrentMode] = useState<'user' | 'environment'>('user');

const handleSwitch = () => {
  if (cameraRef.current) {
    const newMode = cameraRef.current.switchCamera();
    setCurrentMode(newMode);
  }
};

<p>Currently using: {currentMode === 'user' ? 'Front' : 'Back'} camera</p>
```

## Mirror Behavior

The camera view is automatically mirrored when using the front-facing (`user`) camera, just like a selfie in your phone's camera app. The back-facing (`environment`) camera is not mirrored.

**Captured photos** will reflect the mirrored view — what you see is what you get.

## Desktop Behavior

On desktop computers, there's typically only one camera, so the switch button won't have any effect. Use `numberOfCamerasCallback` to detect this and hide the button:

```tsx
{numberOfCameras > 1 && (
  <button onClick={() => cameraRef.current?.switchCamera()}>
    Switch Camera
  </button>
)}
```
