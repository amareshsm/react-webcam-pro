---
sidebar_position: 6
title: Styling
description: How to style the camera component using className, style, and styled-components.
---

# Styling

`react-webcam-pro` provides multiple ways to style the camera container.

## Using `className`

Apply a CSS class to the camera's outer container:

```tsx
<Camera className="my-camera" />
```

```css
.my-camera {
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.2);
}
```

## Using `style`

Apply inline styles directly:

```tsx
<Camera
  style={{
    borderRadius: 16,
    overflow: 'hidden',
    boxShadow: '0 8px 32px rgba(0, 0, 0, 0.2)',
    maxWidth: 640,
    margin: '0 auto',
  }}
/>
```

## Using styled-components

Since `react-webcam-pro` itself uses styled-components internally, you can wrap the Camera in your own styled component:

```tsx
import styled from 'styled-components';
import { Camera } from 'react-webcam-pro';

const StyledCamera = styled(Camera)`
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.2);
`;

// Use it like normal
<StyledCamera ref={cameraRef} facingMode="environment" />
```

:::note
The `className` prop is passed to the outer `<div>` container, not the `<video>` element directly. styled-components' `styled()` wrapper works because it passes a `className` prop.
:::

## Container Structure

The Camera component renders the following DOM structure:

```html
<div>                    <!-- Container: className & style applied here -->
  <div>                  <!-- Wrapper: absolute positioning -->
    <div>...</div>       <!-- Error messages (if any) -->
    <video />            <!-- Camera feed -->
    <canvas />           <!-- Hidden canvas for photo capture -->
  </div>
</div>
```

## Sizing the Camera

### Full-Screen Camera

```tsx
<div style={{ position: 'fixed', inset: 0 }}>
  <Camera aspectRatio="cover" />
</div>
```

### Fixed-Width Camera

```tsx
<div style={{ width: 640, margin: '0 auto' }}>
  <Camera aspectRatio={16 / 9} />
</div>
```

### Responsive Camera

```tsx
<div style={{ width: '100%', maxWidth: 800 }}>
  <Camera
    aspectRatio={4 / 3}
    style={{ borderRadius: 8 }}
  />
</div>
```

## Rounded Camera Viewfinder

A popular pattern is a circular or rounded camera:

```tsx
<Camera
  aspectRatio={1}
  style={{
    borderRadius: '50%',
    overflow: 'hidden',
    width: 300,
    height: 300,
  }}
/>
```

:::info New in v1.0.0
The `className` and `style` props are new in `react-webcam-pro`. They were one of the most [requested features](https://github.com/purple-technology/react-camera-pro/issues/47) in the original package.
:::
