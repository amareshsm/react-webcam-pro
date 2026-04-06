---
sidebar_position: 4
title: Torch / Flashlight
description: How to use the torch (flashlight) feature.
---

# Torch / Flashlight

`react-webcam-pro` supports controlling the device's flashlight (torch) through the camera stream.

## Basic Usage

```tsx
import { useRef, useState } from 'react';
import { Camera, CameraRef } from 'react-webcam-pro';

const App = () => {
  const cameraRef = useRef<CameraRef>(null);
  const [isTorchOn, setIsTorchOn] = useState(false);

  const handleToggleTorch = () => {
    if (cameraRef.current) {
      const newState = cameraRef.current.toggleTorch();
      setIsTorchOn(newState);
    }
  };

  return (
    <div>
      <Camera ref={cameraRef} facingMode="environment" />

      {cameraRef.current?.torchSupported && (
        <button onClick={handleToggleTorch}>
          {isTorchOn ? '🔦 Turn Off' : '🔦 Turn On'}
        </button>
      )}
    </div>
  );
};
```

## Checking Torch Support

Not all devices and browsers support torch control. Always check `torchSupported` before showing torch UI:

```tsx
{cameraRef.current?.torchSupported && (
  <TorchButton onClick={() => cameraRef.current?.toggleTorch()} />
)}
```

## Browser Compatibility

Torch control relies on the `MediaTrackConstraints.advanced` API with the `torch` constraint. Support varies:

| Browser | Torch Support |
|---------|---------------|
| Chrome (Android) | ✅ Supported |
| Chrome (Desktop) | ❌ No torch hardware |
| Safari (iOS) | ❌ Not supported via WebRTC |
| Firefox | ❌ Not supported |
| Samsung Internet | ✅ Supported |

:::note
Torch is only meaningful on the **back (environment)** camera, since front cameras typically don't have a flash.
:::

## How It Works

Under the hood, torch control works by applying advanced constraints to the active media stream track:

```ts
// Simplified internal logic
track.applyConstraints({
  advanced: [{ torch: true }],
});
```

The `torchSupported` property is determined by:
1. Checking if the browser supports `getSupportedConstraints()` 
2. Verifying that `torch` is in the supported constraints
3. Attempting to apply a torch constraint to see if the hardware supports it
