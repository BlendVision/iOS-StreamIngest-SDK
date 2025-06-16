# BVBeautify

BVBeautify is a high-performance iOS framework that provides real-time beautification effects for camera input. It uses Metal for GPU-accelerated image processing to deliver smooth, efficient beautification effects.

## Features

- Real-time beautification effects for camera input
- GPU-accelerated image processing using Metal
- Adjustable skin smoothing and tone enhancement
- Support for both front and back cameras
- Automatic orientation handling with proper image rotation
- High-performance frame processing with detailed profiling
- Frame rate monitoring and performance statistics
- Flexible preview content modes (scaleToFill, scaleAspectFill, scaleAspectFit)
- Efficient memory management with automatic resource cleanup

## Requirements

- iOS 14.0+
- Xcode 14.0+
- Swift 5.0+
- Metal-capable device

## Installation

### Using XCFramework

1. Download the `BVBeautify.xcframework` from the releases
2. Drag the framework into your Xcode project
3. Make sure "Copy items if needed" is checked
4. Add the framework to your target's "Frameworks, Libraries, and Embedded Content" section
5. Set the framework's "Embed" setting to "Embed & Sign"

## Usage

### Basic Setup

1. Import the framework:
```swift
import BVBeautify
```

2. Create a beautifier instance:
```swift
// Enable profiling to get detailed performance measurements
let beautifier = BVBeautifier(enableProfiling: true)

// or, just create the instance without profiling
let beautifier = BVBeautifier()
```

3. Create a render view to display the beautified output:
```swift
let renderView = BVBeautifyRenderView(frame: view.frame, beautifier: beautifier)
view.addSubview(renderView)

// Optionally set the preview content mode
renderView.previewContentMode = .scaleAspectFit
```

4. Set up camera capture and feed frames to the beautifier:
```swift
// In your AVCaptureVideoDataOutputSampleBufferDelegate
func captureOutput(_ output: AVCaptureOutput, 
                  didOutput sampleBuffer: CMSampleBuffer, 
                  from connection: AVCaptureConnection) {
    // Are we using the front camera?
    let isFrontCamera = connection.inputPorts.first?.sourceDevicePosition == .front

    // Update orientation and camera position
    beautifier.setDeviceOrientation(UIDevice.current.orientation)
    beautifier.setActiveCamera(asFront: isFrontCamera)
    renderView.setActiveCamera(asFront: isFrontCamera)
    
    // Process the frame
    beautifier.processFrame(in: sampleBuffer)
}
```

### Adjusting Effects

You can adjust the intensity of the beautification effects:

```swift
// Enable or disable beautification
beautifier.isBeautifyEnabled = true

// Adjust the intensity of skin smoothing effect (0.0 to 1.0)
beautifier.smoothness = 0.5

// Adjust skin tone intensity (0.0 to 1.0)
beautifier.skinToneIntensity = 0.7
```

### Monitoring Performance

Implement the `BVBeautifierDelegate` protocol to monitor frame processing and performance metrics:

```swift
extension YourViewController: BVBeautifierDelegate {
    // Called when a new processed texture is available
    func beautifier(_ beautifier: BVBeautifier,
                   didOutputProcessedTexture texture: Texture) {
        // Use the processed texture
    }
    
    // Called when a new processed sample buffer is available
    func beautifier(_ beautifier: BVBeautifier,
                   didOutputProcessedSampleBuffer sampleBuffer: CMSampleBuffer) {
        // Use the processed sample buffer
    }
    
    // Called when profiling measurements are updated
    func beautifier(_ beautifier: BVBeautifier,
                   didUpdatedProfilingMeasurements measurements: BVBeautifierProfilingMeasurements) {
        // Iterate through all measurements
        for i in 0..<BVBeautifierTimers.numTimers.rawValue {
            let m = measurements[i]
            print(String(format: "\(m.name): %4.1lf ms", m.value * 1000.0))
        }
    }
}

// Add the delegate
beautifier.add(delegate: self)
```

## API Reference

### BVBeautifier

The main class that handles beautification effects.

#### Properties

- `isBeautifyEnabled: Bool` - Controls whether beautification is enabled or disabled
- `smoothness: Float` - Controls the intensity of skin smoothing effect (0.0 to 1.0)
- `skinToneIntensity: Float` - Controls the intensity of skin tone adjustments (0.0 to 1.0)

#### Methods

- `init(enableProfiling: Bool = false)` - Creates a new beautifier instance with optional profiling
- `processFrame(in sampleBuffer: CMSampleBuffer)` - Processes a camera frame
- `add(delegate: BVBeautifierDelegate)` - Adds a delegate to receive updates
- `remove(delegate: BVBeautifierDelegate)` - Removes a delegate
- `printTrackedGFXResources()` - Prints debug information about tracked GFX resources
- `setDeviceOrientation(_ orientation: UIDeviceOrientation)` - Sets the current device orientation
- `setActiveCamera(asFront isFrontCamera: Bool)` - Sets whether the active camera is the front camera

### BVBeautifierDelegate

Protocol for receiving updates from the beautifier.

#### Methods

- `beautifier(_:didOutputProcessedTexture:)` - Called when a new processed texture is available
- `beautifier(_:didOutputProcessedSampleBuffer:)` - Called when a new processed sample buffer is available
- `beautifier(_:didUpdatedProfilingMeasurements:)` - Called when profiling measurements are updated

### BVBeautifierProfilingMeasurements

Structure containing detailed timing measurements for frame processing.

#### Properties

- `Measurement` - A struct containing:
  - `name: String` - The name of the measurement
  - `value: Double` - The measurement value in seconds

#### Methods

- `subscript(index: Int) -> Measurement` - Access measurements by index

Example usage:
```swift
// Called when profiling measurements are updated
func beautifier(_ beautifier: BVBeautifier,
               didUpdatedProfilingMeasurements measurements: BVBeautifierProfilingMeasurements) {
    // Iterate through all measurements
    for i in 0..<BVBeautifierTimers.numTimers.rawValue {
        let m = measurements[i]
        print(String(format: "\(m.name): %4.1lf ms", m.value * 1000.0))
    }
}
```

Available measurements include:
- Frame processing time
- Pre-processing stage time
- GPU processing time
- Post-processing stage time

### BVBeautifyRenderView

A UIView subclass that displays the beautified camera output.

#### Properties

- `previewContentMode: UIView.ContentMode` - Controls how the preview is displayed (scaleToFill, scaleAspectFill, scaleAspectFit)

#### Methods

- `init(frame: CGRect, beautifier: BVBeautifier)` - Creates a new render view
- `setActiveCamera(asFront isFrontCamera: Bool)` - Sets whether the active camera is the front camera

## Performance Considerations

- The framework uses Metal for GPU-accelerated processing to ensure smooth performance
- Frame processing is done on a background thread to prevent UI blocking
- Memory management is handled automatically with proper resource cleanup
- Performance profiling can be enabled to monitor processing times
- The framework automatically handles device orientation and camera position changes

## License

Copyright © 2025 KK Company. All rights reserved.
