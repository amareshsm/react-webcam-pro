---
sidebar_position: 1
title: Installation
description: How to install react-webcam-pro in your React project.
---

# Installation

## Prerequisites

Before installing `react-webcam-pro`, make sure you have:

- **Node.js** ≥ 16
- **React** ≥ 16.8 (hooks support required)
- **styled-components** v5 or v6

:::info WebRTC Requirement
Camera access via WebRTC is only available on **secure origins** (HTTPS).  
You can develop and test on `localhost` in Chrome, but Safari requires HTTPS even for localhost.

See [browser compatibility](https://caniuse.com/#feat=stream) for full details.
:::

## Install via npm

```bash
npm install react-webcam-pro
```

## Install via yarn

```bash
yarn add react-webcam-pro
```

## Install via pnpm

```bash
pnpm add react-webcam-pro
```

## Peer Dependencies

`react-webcam-pro` requires the following peer dependencies to be installed in your project:

| Package              | Version                                   |
| -------------------- | ----------------------------------------- |
| `react`              | `^16.8.0 \|\| ^17.0.0 \|\| ^18.0.0 \|\| ^19.0.0` |
| `react-dom`          | `^16.8.0 \|\| ^17.0.0 \|\| ^18.0.0 \|\| ^19.0.0` |
| `styled-components`  | `^5.0.0 \|\| ^6.0.0`                      |

If you don't already have them, install them alongside the package:

```bash
npm install react-webcam-pro react react-dom styled-components
```

## TypeScript Support

`react-webcam-pro` is written in TypeScript and ships with built-in type declarations. No additional `@types` packages are needed.

## Verify Installation

After installing, you can verify everything is set up correctly by creating a simple test component:

```tsx
import { Camera } from 'react-webcam-pro';

function TestCamera() {
  return <Camera />;
}
```

If this compiles without errors, you're ready to go!

## Next Steps

Head over to the [Quick Start](/docs/getting-started/quick-start) guide to build your first camera app.

:::tip Community Support
We're actively fixing open issues from the original `react-camera-pro` repo. If you encounter a bug or need something fixed urgently, [create an issue](https://github.com/amareshsm/react-webcam-pro/issues/new) in our repository — it will be prioritized and addressed quickly.
:::
