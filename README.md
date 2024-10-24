# BlendVision Stream Ingest iOS SDK - Beta version
BlendVision stream ingest SDK is a powerful streaming ingestion library for live video streaming on iOS platform.

## Installation
### Using Swift Package Manager
[Swift Package Manager](https://www.swift.org/documentation/package-manager/) is a tool for managing the distribution of Swift frameworks. It integrates with the Swift build system to automate the process of downloading, compiling, and linking dependencies.

#### Using Xcode
To integrate the development version using Xcode 14, follow these steps:
1. Open your Project file in Xcode.
2. Go to Project > Package Dependencies.
3. Click the "+" button to add a package.
4. In the URL field, enter:
```swift
https://github.com/BlendVision/iOS-StreamIngest-SDK.git
```
5. Select the `dev` branch in the version rules to use the latest development version.
  - In the "Dependency Rule" section, choose "Branch" and enter `dev`.
  - This will integrate the SDK from the latest development branch.

> **Note**: This is the development version of the SDK and may include unstable or experimental features. It is intended for testing and feedback purposes only. For production use, please wait for the official stable release.

## Our Examples
See our [examples](https://github.com/BlendVision/iOS-StreamIngest-Samples) for more details. There, you can find simple applications, including BasicStreamIngesting for iOS.
