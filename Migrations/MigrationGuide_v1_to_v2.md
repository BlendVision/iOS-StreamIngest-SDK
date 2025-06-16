# Migration Guide - v1 to v2

This document provides a detailed overview of the changes made in the new version compared to the old version. The changes are categorized into new additions, property modifications, method updates, and notable changes.

## New Additions
- **`StreamIngestConfig` Class:**
  - **Description**: The `StreamIngestConfig` class is introduced as a configuration object used for initializing and configuring a stream ingestion process.
  - **Usage**: This class stores essential information, such as the license key required for the stream ingest process.

## Property Modifications
- **Removed Properties:**
  - **`authorizer`:** The `authorizer` property has been removed in the new version.

- **Modified Properties:**
  - **`cameraPreviewView`:**
    - **Old Type**: `UIView`
    - **New Type**: `CameraPreviewView`
    - **Description**: This change indicates a shift towards using a more specialized view for handling camera previews.

## Method Updates
- **StreamIngest Initialization:**
  - **Change**: The initializer of the `StreamIngest` class has been made private.
  - **New Approach**: Instances of `StreamIngest` must now be created using the `create(with:)` method.
  - **Usage**:
    ```swift
    let config = StreamIngestConfig(key: licenseKey)
    guard let streamIngest = try await StreamIngest.create(with: config) else { return }
    ```
- **StreamIngest Authorization:**
  - **Change**: The `authorizer` property has been removed in the new version.
  - **New Approach**: Authorization is now required using the `requestAuthentication(completion:)` method.
  - **Usage**:
    ```swift
    streamIngest.requestAuthentication { error in
      if let error {
          debugPrint("Authority error = \(error)")
      } else {
          debugPrint("StreamIngest SDK authority success")
      }
    }
    ```
- **EffectStreamIngest CameraPreviewView Creation:**
  - **Change**: The new version introduces the `CameraPreviewView` class, replacing the generic `UIView` used for the camera preview in the old version.
  - **New Approach**: Using the `createCameraView(frame:aspectRatio:)` method to create the instance of `CameraPreviewView`.
  - **Usage**:
    ```swift
    guard let previewView = effectStreamIngest?.createCameraView(
          frame: .zero,
          aspectRatio: 16.0 / 9.0
      ) else { return }
      
      previewView.translatesAutoresizingMaskIntoConstraints = false
      view.addSubview(previewView)

      // Using Auto Layout to arrange the views is recommended
      NSLayoutConstraint.activate([
          previewView.leadingAnchor.constraint(equalTo: view.leadingAnchor),
          previewView.trailingAnchor.constraint(equalTo: view.trailingAnchor),
          previewView.topAnchor.constraint(equalTo: view.topAnchor),
          previewView.bottomAnchor.constraint(equalTo: view.bottomAnchor)
    ])
    ```
- **CameraPreviewView Size Adjustment:**
  - **Change**: N/A
  - **Approach**: Using the `adjustRenderView(to:)` method for adjusting the size of `CameraPreviewView`.
  - **Usage**:
    ```swift
    override func viewDidLayoutSubviews() {
        super.viewDidLayoutSubviews()
        cameraPreviewView?.adjustRenderView()
    }
    
    override func viewWillTransition(to size: CGSize, with coordinator: UIViewControllerTransitionCoordinator) {
        cameraPreviewView?.adjustRenderView(to: size)
    }
    ```    

## Notable Changes
- **Introduction of `CameraPreviewView`:**
  - **Description**: The new version introduces the `CameraPreviewView` class, replacing the generic `UIView` used for the camera preview in the old version.
  - **Implication**: This suggests a shift towards a more specialized and possibly more functional view for camera previews, potentially offering enhanced features and better integration with camera-related functionalities.

## Summary of Changes
1. **New Additions:**
   - Added the `StreamIngestConfig` class for better configuration management.

2. **Property Modifications:**
   - Removed the `authorizer` property.
   - Changed the type of `cameraPreviewView` from `UIView` to `CameraPreviewView`.

3. **Method Updates:**
   - Made the `StreamIngest` initializer private and introduced the `create(with:)` method for instance creation.
   - Updated the authorization process to use the `requestAuthentication(completion:)` method instead of the authorizer property.
   - Introduced the `createCameraView(frame:aspectRatio:)` method for creating a `CameraPreviewView`.
   - Introduced the `adjustRenderView(to:)` method for adjusting the size of `CameraPreviewView`.

4. **Specialized View:**
   - Introduced `CameraPreviewView` to handle camera previews more effectively.

This migration document aims to provide a clear and concise overview of the changes between the old and new versions, ensuring a smooth transition and understanding of the new features and modifications.
