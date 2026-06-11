# YouTubePlayer

Embed and control YouTube videos in your iOS applications.

This fork is maintained as a Swift Package Manager-only package. CocoaPods and Carthage integration have been removed.

**0.7.0 Update:** [`WKWebView` breaking changes](#breaking-changes)

## Installation

### Swift Package Manager

In Xcode:

1. Open your project.
2. Go to File > Add Package Dependencies.
3. Enter this repository URL.
4. Select the required version, branch, or commit.
5. Add the `YouTubePlayer` package product to your app target.

Minimum supported iOS version: 15.6.

> Note: SwiftPM cannot express iOS 15.6 exactly in `Package.swift`, so the package manifest declares `.iOS(.v15)`. Treat iOS 15.6 as the intended minimum runtime for this fork.

If this fork has not published a release tag yet, select the branch or commit you want Xcode to consume.

## Example

```Swift
// Import Swift module
import YouTubePlayer
```

Build and lay out the view however you wish, whether in Interface Builder with an outlet or programmatically.

```Swift
@IBOutlet var videoPlayer: YouTubePlayerView!
```

```Swift
// init YouTubePlayerView w/ playerFrame rect (assume playerFrame declared)
var videoPlayer = YouTubePlayerView(frame: playerFrame)
```

Give the player a video to load, whether from ID or URL.

```Swift
// Load video from YouTube ID
videoPlayer.loadVideoID("nfWlot6h_JM")
```

```Swift
// Load video from YouTube URL
let myVideoURL = URL(string: "https://www.youtube.com/watch?v=wQg3bXrVLtg")
videoPlayer.loadVideoURL(myVideoURL!)
```

## Controlling YouTubePlayerView

Each `YouTubePlayerView` has methods for controlling the player (play, pause, seek, change video, etc.). They are:

* `func loadVideoURL(_ videoURL: URL)`
* `func loadVideoID(_ videoID: String)`
* `func loadPlaylistID(_ playlistID: String)`
* `func mute()`
* `func unMute()`
* `func play()`
* `func pause()`
* `func stop()`
* `func clear()`
* `func seekTo(_ seconds: Float, seekAhead: Bool)`
* `func previousVideo()`
* `func nextVideo()`
* `func getCurrentTime(completion: ((Double?) -> Void)?)`
* `func getDuration(completion: ((Double?) -> Void)?)`

Please note that calls to all but the load methods will result in a JavaScript runtime error if they are called before the player is ready. The player will not be ready until shortly after a call to either `loadVideoURL(_:)` or `loadVideoID(_:)`. You can check the readiness of the player at any time by checking its `ready: Bool` property. These functions run asynchronously, so it is not guaranteed that a call to a play function will be safe if it immediately follows a call to a load function.

You can also use the `YouTubePlayerDelegate` method `playerReady(_:)` to ensure code is executed immediately when the player becomes ready.

## Responding to events

[YouTube's iFrame player](https://developers.google.com/youtube/iframe_api_reference) emits certain events based on the lifecycle of the player. The `YouTubePlayerDelegate` outlines these methods that get called during a player's lifecycle. They are:

* `func playerReady(_ videoPlayer: YouTubePlayerView)`
* `func playerStateChanged(_ videoPlayer: YouTubePlayerView, playerState: YouTubePlayerState)`
* `func playerQualityChanged(_ videoPlayer: YouTubePlayerView, playbackQuality: YouTubePlaybackQuality)`
* `func playerEncounteredError(_ videoPlayer: YouTubePlayerView, errorMessage: String)`

*Side note:* All these delegate methods are optional, which means that you can implement none, all, or some of them in your delegate class.

## Breaking Changes

**SwiftPM-only fork**

CocoaPods and Carthage integration have been removed. Install this fork with Swift Package Manager. The intended minimum runtime is iOS 15.6, while `Package.swift` declares `.iOS(.v15)` because SwiftPM does not expose a `.v15_6` platform case.

**0.7.0**

Transitioning from `UIWebView` (deprecated) to `WKWebView` required changing player calls which return values to be asynchronous. If you upgrade to 0.7.0, you will need to replace any call to either `getCurrentTime()` and `getDuration()` with its asynchronous equivalent, [documented above](#controlling-youtubeplayerview).
