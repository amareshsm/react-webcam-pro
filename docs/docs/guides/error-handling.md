---
sidebar_position: 3
title: Error Handling
description: How to handle camera errors and customize error messages.
---

# Error Handling

Camera access can fail for many reasons — missing permissions, no camera hardware, unsupported browsers, etc. `react-camera-web` handles these gracefully and lets you customize the user experience.

## Built-in Error Messages

The Camera component automatically displays error messages when something goes wrong. By default, these are:

| Scenario | Default Message |
|----------|----------------|
| No camera accessible | *"No camera device accessible. Please connect your camera or try a different browser."* |
| Permission denied | *"Permission denied. Please refresh and give camera permission."* |
| Can't switch camera | *"It is not possible to switch camera to different one because there is only one video device accessible."* |
| Canvas not supported | *"Canvas is not supported."* |

## Customizing Error Messages

Override any or all messages using the `errorMessages` prop:

```tsx
<Camera
  errorMessages={{
    noCameraAccessible: 'No camera found. Please connect a camera.',
    permissionDenied: 'Please allow camera access to continue.',
  }}
/>
```

You only need to provide the messages you want to change — the rest will use defaults:

```tsx
// Only customize one message
<Camera
  errorMessages={{
    permissionDenied: 'Camera blocked! Check your browser settings.',
  }}
/>
```

## Error Scenarios

### No Camera Found

Displayed when `navigator.mediaDevices.getUserMedia` fails because no video input device is available. This typically happens on:
- Desktop computers without a webcam
- Devices where the camera is disabled in system settings

### Permission Denied

Displayed when the user denies the browser's camera permission prompt, or when the permission was previously denied.

Common causes:
- User clicked "Deny" on the permission popup
- Camera permission was previously denied and cached by the browser
- The page is not served over HTTPS (except localhost)

### Canvas Not Supported

Extremely rare. Displayed when the browser doesn't support the HTML5 Canvas API, which is required for capturing photos.

## Handling Errors in Code

The `takePhoto()` and `switchCamera()` methods can throw errors. Wrap them in try-catch:

```tsx
const handleCapture = () => {
  try {
    const photo = cameraRef.current?.takePhoto();
    setImage(photo as string);
  } catch (error) {
    alert(`Failed to take photo: ${error.message}`);
  }
};
```

## WebRTC Requirements

:::warning HTTPS Required
Camera access via WebRTC requires a **secure origin** (HTTPS). Browsers will block `getUserMedia` on insecure origins.

**Exceptions:**
- `http://localhost` works in Chrome and Firefox
- `http://127.0.0.1` works in Chrome and Firefox
- Safari requires HTTPS even for localhost
:::

## Detecting Browser Support

You can check if the browser supports camera access before rendering the component:

```tsx
const isCameraSupported = 
  typeof navigator !== 'undefined' && 
  navigator.mediaDevices && 
  typeof navigator.mediaDevices.getUserMedia === 'function';

if (!isCameraSupported) {
  return <p>Your browser doesn't support camera access.</p>;
}

return <Camera ref={cameraRef} />;
```
