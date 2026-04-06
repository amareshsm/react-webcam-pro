---
sidebar_position: 7
title: Using in an iframe
description: How to use react-webcam-pro inside an iframe.
---

# Using in an iframe

If your camera component is rendered inside an `<iframe>`, you need to explicitly grant camera permissions.

## Allowing Camera Access

Add the `allow` attribute to your iframe:

```html
<iframe
  src="https://your-camera-app.com"
  allow="camera;"
  width="640"
  height="480"
/>
```

Without the `allow="camera;"` attribute, the browser will block `getUserMedia` inside the iframe, even if the parent page has camera access.

## Permissions Policy

Modern browsers use the [Permissions Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Permissions-Policy) (formerly Feature Policy) to control which features iframes can use.

For the camera to work in an iframe, the `camera` feature must be allowed:

```html
<!-- Allow camera for same-origin iframes -->
<iframe src="/camera-view" allow="camera;"></iframe>

<!-- Allow camera for specific origin -->
<iframe src="https://camera.example.com" allow="camera https://camera.example.com;"></iframe>

<!-- Allow camera for any origin (use with caution) -->
<iframe src="https://third-party.com" allow="camera *;"></iframe>
```

## Cross-Origin Considerations

If the iframe loads content from a **different origin**, you also need:

1. The parent page must allow the `camera` feature in the iframe via the `allow` attribute.
2. The iframe content must be served over **HTTPS**.
3. The user will still see the browser's camera permission prompt.

## Common Issues

### Camera Not Working in iframe

If the camera doesn't work inside an iframe:

1. ✅ Check that `allow="camera;"` is set on the `<iframe>` tag
2. ✅ Ensure the iframe content is served over HTTPS
3. ✅ Verify that no `Permissions-Policy` HTTP header is blocking camera access
4. ✅ Check browser dev tools console for permission errors

### Safari-Specific Notes

Safari has stricter iframe permission policies:
- Camera access in cross-origin iframes may require user gesture
- The `allow` attribute must explicitly include `camera`
- Safari may block camera access in iframes that aren't visible
