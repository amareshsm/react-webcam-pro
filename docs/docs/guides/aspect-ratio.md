---
sidebar_position: 2
title: Aspect Ratio
description: How to control the camera view's aspect ratio.
---

# Aspect Ratio

The `aspectRatio` prop controls how the camera view is sized within its container.

## Cover Mode (Default)

By default, the camera fills the entire parent container:

```tsx
<Camera aspectRatio="cover" />
```

In cover mode, the component uses `position: absolute` with `top: 0; bottom: 0; left: 0; right: 0` to fill its parent. Make sure the parent has a defined size:

```tsx
<div style={{ position: 'relative', width: '100%', height: '100vh' }}>
  <Camera aspectRatio="cover" />
</div>
```

## Numeric Aspect Ratios

Pass a number to set a specific aspect ratio. The value is `width / height`:

```tsx
// 16:9 widescreen
<Camera aspectRatio={16 / 9} />

// 4:3 classic
<Camera aspectRatio={4 / 3} />

// 1:1 square
<Camera aspectRatio={1} />

// 3:2
<Camera aspectRatio={3 / 2} />
```

When using a numeric aspect ratio, the component:
- Takes the full width of its parent
- Calculates height using CSS `padding-bottom` to maintain the ratio
- Positions itself relatively within the container

## Photo Aspect Ratio

The captured photo always matches the **displayed** aspect ratio. If the camera's native resolution (e.g., 1920×1080 = 16:9) differs from the displayed ratio (e.g., 1:1), the photo is automatically cropped to match.

This means what the user sees in the viewfinder is exactly what they'll get in the photo.

## Responsive Behavior

The camera view is always responsive to its container width. With a numeric aspect ratio, the height adjusts automatically:

```tsx
// Camera takes up 100% width of its container
// Height is calculated as width / aspectRatio
<div style={{ maxWidth: 640, margin: '0 auto' }}>
  <Camera aspectRatio={16 / 9} />
</div>
```

## Common Aspect Ratios

| Ratio | Value | Use Case |
|-------|-------|----------|
| 16:9 | `16 / 9` | Widescreen, modern videos |
| 4:3 | `4 / 3` | Classic photos, documents |
| 1:1 | `1` | Profile photos, social media |
| 3:2 | `3 / 2` | DSLR photos |
| 9:16 | `9 / 16` | Vertical / portrait video |
| Cover | `'cover'` | Fill the container completely |
