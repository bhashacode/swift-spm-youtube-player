# Release Notes

## SwiftPM-only migration

- Removed CocoaPods support and legacy package-manager installation guidance.
- Removed Carthage installation guidance; this fork is Swift Package Manager-only.
- Added a root `Package.swift` with a `YouTubePlayer` library product.
- Moved library source into `Sources/YouTubePlayer/`.
- Moved the YouTube player HTML file into `Sources/YouTubePlayer/Resources/` and load it through SwiftPM package resources.
- Intended minimum runtime: iOS 15.6.
- SwiftPM manifest platform declaration: `.iOS(.v15)` because SwiftPM cannot express iOS 15.6 exactly.
