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
- **`StreamIngest` Initialization:**
  - **Change**: The initializer of the `StreamIngest` class has been made private.
  - **New Approach**: Instances of `StreamIngest` must now be created using the `create(with:)` method.
  - **Usage**:
    ```swift
    let config = StreamIngestConfig(key: licenseKey)
    guard let streamIngest = try await StreamIngest.create(with: config) else { return }
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

4. **Specialized View:**
   - Introduced `CameraPreviewView` to handle camera previews more effectively.

This migration document aims to provide a clear and concise overview of the changes between the old and new versions, ensuring a smooth transition and understanding of the new features and modifications.
