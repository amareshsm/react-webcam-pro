---
sidebar_position: 1
title: Migrating from react-camera-pro
description: Step-by-step guide for migrating from react-camera-pro to react-webcam-pro.
---

# Migrating from react-camera-pro

`react-webcam-pro` is a community-maintained fork of [`react-camera-pro`](https://github.com/purple-technology/react-camera-pro) by [Purple Technology](https://github.com/purple-technology). The original package has not been actively maintained for over 2 years, leaving many users with unresolved issues.

**`react-webcam-pro` is a drop-in replacement** — the API is fully backward compatible. This guide walks you through the migration step by step.

---

## Why Migrate?

The original `react-camera-pro` has several unresolved issues:

| Issue | Status in react-camera-pro | Status in react-webcam-pro |
|-------|---------------------------|---------------------------|
| React 19 support | ❌ [Not supported](https://github.com/purple-technology/react-camera-pro/issues/70) | ✅ Fixed |
| styled-components v6 DOM warnings | ❌ [Open issue](https://github.com/purple-technology/react-camera-pro/issues/48) | ✅ Fixed |
| `errorMessages` prop required | ❌ [Required even for defaults](https://github.com/purple-technology/react-camera-pro/issues/63) | ✅ Optional |
| `className` / `style` props | ❌ [Not available](https://github.com/purple-technology/react-camera-pro/issues/47) | ✅ Added |
| `videoSourceDeviceId` ignored | ❌ [Broken in environment mode](https://github.com/purple-technology/react-camera-pro/issues/62) | ✅ Fixed |
| Camera switching with device ID | ❌ [Broken](https://github.com/purple-technology/react-camera-pro/issues/69) | ✅ Fixed |
| Modern toolchain | ❌ TypeScript 3.7, Rollup 1 | ✅ TypeScript 5, Rollup 4 |
| Test suite | ❌ No tests | ✅ 60+ tests |

---

## Step 1: Replace the Package

```bash
# Uninstall the old package
npm uninstall react-camera-pro

# Install the new package
npm install react-webcam-pro
```

Or with yarn:

```bash
yarn remove react-camera-pro
yarn add react-webcam-pro
```

Or with pnpm:

```bash
pnpm remove react-camera-pro
pnpm add react-webcam-pro
```

---

## Step 2: Update Imports

This is the **only required change**. Update your import statements:

```diff
- import { Camera, CameraType } from 'react-camera-pro';
+ import { Camera, CameraType } from 'react-webcam-pro';
```

If you have multiple files, you can do a find-and-replace across your project:

- **Find:** `react-camera-pro`
- **Replace:** `react-webcam-pro`

---

## Step 3: That's It! ✅

Your application should now work exactly as before. All props, methods, and types are backward compatible.

---

## Optional Improvements

While not required, here are some improvements you can make now:

### Use `CameraRef` Instead of `CameraType`

`CameraType` is still exported for backward compatibility, but `CameraRef` is the recommended type:

```diff
- import { Camera, CameraType } from 'react-webcam-pro';
- const camera = useRef<CameraType>(null);
+ import { Camera, CameraRef } from 'react-webcam-pro';
+ const camera = useRef<CameraRef>(null);
```

### Remove Redundant `errorMessages`

If you were passing `errorMessages` only because it was required, you can now remove it:

```diff
  <Camera
    ref={camera}
    facingMode="environment"
-   errorMessages={{
-     noCameraAccessible: 'No camera device accessible. Please connect your camera or try a different browser.',
-     permissionDenied: 'Permission denied. Please refresh and give camera permission.',
-     switchCamera: 'It is not possible to switch camera to different one because there is only one video device accessible.',
-     canvas: 'Canvas is not supported.',
-   }}
  />
```

The defaults are the same, so the behavior won't change.

### Add `className` or `style`

You can now style the camera container directly:

```diff
  <Camera
    ref={camera}
+   className="camera-viewfinder"
+   style={{ borderRadius: 12, overflow: 'hidden' }}
  />
```

---

## Detailed API Comparison

### Props

| Prop | react-camera-pro | react-webcam-pro | Notes |
|------|-----------------|-----------------|-------|
| `facingMode` | ✅ Optional | ✅ Optional | No change |
| `aspectRatio` | ✅ Optional | ✅ Optional | No change |
| `numberOfCamerasCallback` | ✅ Optional | ✅ Optional | No change |
| `videoSourceDeviceId` | ✅ Optional | ✅ Optional | Fixed: now takes priority |
| `errorMessages` | ⚠️ **Required** | ✅ Optional | Now optional with defaults |
| `videoReadyCallback` | ✅ Optional | ✅ Optional | No change |
| `className` | ❌ Not available | ✅ Optional | **New** |
| `style` | ❌ Not available | ✅ Optional | **New** |

### Ref Methods

| Method/Property | react-camera-pro | react-webcam-pro | Notes |
|----------------|-----------------|-----------------|-------|
| `takePhoto(type?)` | ✅ | ✅ | No change |
| `switchCamera()` | ✅ | ✅ | No change |
| `getNumberOfCameras()` | ✅ | ✅ | No change |
| `toggleTorch()` | ✅ | ✅ | No change |
| `torchSupported` | ✅ | ✅ | No change |

### Exports

| Export | react-camera-pro | react-webcam-pro | Notes |
|--------|-----------------|-----------------|-------|
| `Camera` | ✅ | ✅ | No change |
| `CameraType` | ✅ | ✅ (deprecated) | Kept as alias for `CameraRef` |
| `CameraProps` | ✅ | ✅ | No change |
| `CameraRef` | ❌ | ✅ | **New** — recommended ref type |

### Peer Dependencies

| Dependency | react-camera-pro | react-webcam-pro |
|-----------|-----------------|-----------------|
| `react` | `^18.3.1` | `^16.8.0 \|\| ^17.0.0 \|\| ^18.0.0 \|\| ^19.0.0` |
| `react-dom` | `^18.3.1` | `^16.8.0 \|\| ^17.0.0 \|\| ^18.0.0 \|\| ^19.0.0` |
| `styled-components` | `^5.1.34` | `^5.0.0 \|\| ^6.0.0` |

---

## Bug Fixes You Get Automatically

By migrating, you automatically get fixes for these known issues:

### DOM Warnings with styled-components v6

**Issue:** [#48](https://github.com/purple-technology/react-camera-pro/issues/48)

`react-camera-pro` passed `mirrored` and `aspectRatio` directly to DOM elements, causing React warnings:
```
Warning: React does not recognize the `mirrored` prop on a DOM element.
```

**Fix:** `react-webcam-pro` uses styled-components [transient props](https://styled-components.com/docs/api#transient-props) (`$mirrored`, `$aspectRatio`) that don't get passed to the DOM.

### `videoSourceDeviceId` Ignored in Environment Mode

**Issue:** [#62](https://github.com/purple-technology/react-camera-pro/issues/62), [#69](https://github.com/purple-technology/react-camera-pro/issues/69)

When using `facingMode="environment"`, the component would auto-detect environment cameras and **override** the explicitly set `videoSourceDeviceId`.

**Fix:** When `videoSourceDeviceId` is provided, it always takes priority over auto-detection.

### Deprecated WebRTC Fallbacks Removed

`react-camera-pro` included fallback code for `navigator.getUserMedia`, `navigator.webkitGetUserMedia`, `navigator.mozGetUserMedia`, and `navigator.msGetUserMedia` — all of which are long-deprecated browser APIs.

`react-webcam-pro` uses only the modern `navigator.mediaDevices.getUserMedia` API.

---

## Troubleshooting

### TypeScript Errors After Migration

If you see TypeScript errors, make sure you've updated all import paths. Search your codebase for any remaining references to `react-camera-pro`:

```bash
grep -r "react-camera-pro" --include="*.ts" --include="*.tsx" src/
```

### Peer Dependency Warnings

If you see peer dependency warnings, ensure your `react`, `react-dom`, and `styled-components` versions are within the supported ranges:

- `react` / `react-dom`: `^16.8.0 || ^17.0.0 || ^18.0.0 || ^19.0.0`
- `styled-components`: `^5.0.0 || ^6.0.0`

---

## Original Package Credits

`react-webcam-pro` is built on the work of:

- [**Martin Urban**](https://github.com/murbanowicz) — Original author of react-camera-pro
- [**Purple Technology**](https://github.com/purple-technology) — Original maintainers
- All [contributors](https://github.com/purple-technology/react-camera-pro/graphs/contributors) to the original package

We're grateful for their work and committed to continuing the project for the community.
