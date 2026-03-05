# 🎬 KMPPlayer — Kotlin Multiplatform Media Player

[![Maven Central](https://img.shields.io/maven-central/v/io.github.rufenkhokhar/KMP-Player)](https://central.sonatype.com/artifact/io.github.rufenkhokhar/KMP-Player)
![Kotlin](https://img.shields.io/badge/Kotlin-Multiplatform-7F52FF?logo=kotlin&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green.svg)
![Platform](https://img.shields.io/badge/Platforms-Android%20%7C%20iOS-blue)
![Compose](https://img.shields.io/badge/Jetpack-Compose-4285F4?logo=jetpackcompose&logoColor=white)
![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)
![Issues](https://img.shields.io/github/issues/rufenkhokhar/KMP-Player)
![Stars](https://img.shields.io/github/stars/rufenkhokhar/KMP-Player?style=social)

**KMPPlayer** is a Kotlin Multiplatform media playback library that delivers a unified, reactive interface to control media playback across **Android** and **iOS** platforms. Internally, it leverages **ExoPlayer** on Android and **AVPlayer** on iOS, while exposing a clean Kotlin API usable from shared code and Jetpack Compose.

---

## ✨ Features

- 🧩 Unified playback interface (`KMPPlayerState`)
- 📁 Supports local files & remote media URLs
- 🎮 Built-in media controls: play, pause, stop, loop, seek
- 📡 Reactive `StateFlow`-based event stream (`KMPPlayerEvent`)
- 🎚 Volume & playback speed control
- 📍 Position & duration tracking
- 🎨 Jetpack Compose support (`KMPPlayer`)
- ⚙️ Access native players: ExoPlayer (Android), AVPlayer (iOS)
- 🧼 Proper resource cleanup with `destroy()`

---

## 📦 Installation

### 1. Add Maven Repository

Published to **Maven Central**:

```kotlin
repositories {
    mavenCentral()
}
```

---

### 2. Add the Dependency

For Kotlin Multiplatform (shared code):

```kotlin
kotlin {
    sourceSets {
        commonMain.dependencies  {
           implementation("io.github.rufenkhokhar:KMP-Player:+")
        }
    }
}
```

Or for Android-only usage:

```kotlin
dependencies {
    implementation("io.github.rufenkhokhar:KMP-Player:+")
}
```

---

## ⚡ Quick Start (Jetpack Compose)

```kotlin
val playerState = rememberKMPPlayerState()

KMPPlayer(
    state = playerState,
    showControls = true,
    modifier = Modifier
        .fillMaxSize()
        .background(Color.Black)
)

LaunchedEffect(Unit) {
    // Load media
    playerState.setFileUrl("https://www.sample-videos.com/video321/mp4/360/big_buck_bunny_360p_20mb.mp4")

    // Configure playback
    playerState.setVolume(0.8f)          // 80% volume
    playerState.setPlaybackSpeed(1.25f)  // 1.25x speed
    playerState.setVideoLoop(true)       // enable looping

    // Start playback
    playerState.play()

    // Observe playback events
    playerState.observePlayerEvent().collect { event ->
        when (event) {
            is KMPPlayerEvent.Playing -> {
                println("Playing: ${event.currentPosition} / ${event.duration}")
            }
            KMPPlayerEvent.Paused    -> println("Paused")
            KMPPlayerEvent.Stop      -> println("Stopped")
            KMPPlayerEvent.Buffering -> println("Buffering…")
            KMPPlayerEvent.Ideal     -> println("Idle")
            KMPPlayerEvent.Ended     -> println("Playback ended")
            is KMPPlayerEvent.Error  -> println("Error: ${event.message}")
        }
    }
}
```

---

## 🧩 API Reference

### 🔧 `KMPPlayerState`

```kotlin
@ExperimentalMultiplatform
interface KMPPlayerState {
    fun play()
    fun stop()
    fun pause()
    fun isPlaying(): Boolean

    fun setLocalFile(absolutePath: String)
    fun setFileUrl(url: String)
    fun setVideoLoop(loop: Boolean)

    fun observePlayerEvent(): StateFlow<KMPPlayerEvent>
    fun getPlatformPlayer(): Any?
    fun destroy()

    fun setVolume(volume: Float)
    fun getVolume(): Float

    fun setPlaybackSpeed(speed: Float)

    fun getCurrentPosition(): Long
    fun getDuration(): Long
    fun seekTo(position: Long)
}
```

### 📡 `KMPPlayerEvent`

```kotlin
sealed interface KMPPlayerEvent {
    data class Playing(
        val currentPosition: Long = 0L,
        val duration: Long = 0L
    ) : KMPPlayerEvent

    data object Paused : KMPPlayerEvent
    data object Stop : KMPPlayerEvent
    data object Buffering : KMPPlayerEvent
    data object Ideal : KMPPlayerEvent
    data object Ended : KMPPlayerEvent

    data class Error(val message: String) : KMPPlayerEvent
}
```

---

## 💡 Tips

- Use `setVideoLoop(true)` for infinite looping
- Use `setVolume(0.0f–1.0f)` to control playback volume
- Use `setPlaybackSpeed(speed)` to adjust playback rate
- Use `getCurrentPosition()` + `getDuration()` for progress tracking
- Use `seekTo(ms)` to scrub or skip
- Always call `destroy()` to release player resources when not in use
- For advanced control, use `getPlatformPlayer()` and cast to native types

---

## 📄 License

MIT License  
© 2025 [Rufen Khokhar](https://github.com/rufenkhokhar)

---

## 🤝 Contributing

Pull requests and feature ideas are welcome!  
Open an issue to start the discussion.

---

## 👤 Maintainer

**Rufen Khokhar**  
📧 rufankhokhar@gmail.com  
🔗 [github.com/rufenkhokhar](https://github.com/rufenkhokhar)
