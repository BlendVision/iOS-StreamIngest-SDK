# Migration Guide - v2 to v3

## 1. Camera Preview View Handling

### Old Version
- Used `adjustRenderView()` and `adjustRenderView(to:)` in `viewDidLayoutSubviews` and `viewWillTransition(to:with:)` to manage camera preview layout.
- Camera attachment was implicit, with no explicit activation.

### New Version
- Removed `adjustRenderView()` and related calls; preview layout now handled entirely via Auto Layout.
- Introduced `activateCamera(isFrontCamera:)` in `setupCameraView(_:)` to explicitly activate the camera.
- Added `rotateCamera()` to support camera switching.

### Impact
- Simplifies preview layout management.
- Improves reliability of camera switching and orientation handling.

## 2. `EffectStreamIngestDelegate` Changes

### Old Version
- Used `effectStreamIngest(_:didChange:)` for effect loading.
- Used `effectStreamIngestSwitchToUnfiltered(_:)` to switch to unfiltered mode, which included reattaching the camera and recreating the preview.

### New Version
- Replaced with `effectStreamIngest(_:didOutputVideo:)` and `effectStreamIngest(_:didOutputAudio:)`, which handle raw sample buffer processing directly.
- Removed logic related to unfiltered mode.

### Impact
- Simplifies delegate responsibilities.
- Focus shifts to handling sample buffers, enabling more flexible processing pipelines.

## 3. Removal of `enableSkinSmoothFaceOnly` and Obsolete Code

### Old Version
- Contained commented-out line `effectStreamIngest?.filterConfig.enableSkinSmoothFaceOnly = true`.

### New Version
- Removed `enableSkinSmoothFaceOnly` setting.

### Impact
- Skin smoothing configuration may have been centralized or replaced.
- Codebase is cleaner and easier to maintain without obsolete and commented-out code.

## Removed APIs

The following APIs have been removed from the SDK:

### EffectStreamIngest

```swift
// Removed initializer
public convenience init(streamIngest: StreamIngest, 
    config: EffectStreamIngestConfig = EffectStreamIngestConfig(), 
    videoRawDataFrameSize: CGSize = CGSize(width: 720, height: 1280))

// Removed method
public func createCameraView(frame: CGRect, aspectRatio ratio: CGFloat) -> CameraPreviewView?
```

### StreamIngestAuthorizer

```swift
// Removed method
public func requestAuthentication(completion: ((Error?) -> Void)? = nil)
```

Note: These APIs have been permanently removed. Please use the following alternatives:
- For camera view creation, use `createCameraView(mode:)` instead
- For authentication, use `StreamIngest.create()` method
- For stream ingest initialization, use `init(streamIngest:)` without the videoRawDataFrameSize parameter
