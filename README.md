# Exercise 4 - Add Media Session

## _QuizActivity.java_
#### 1. Create a method to initialize the `MediaSession`. It should create the `MediaSessionCompat` object, set the flags for external clients, set the available actions you want to support, and start the session.[[code][1]]

```java
    private void initializeMediaSession() {
        //Create a MediaSessionCompat
        mMediaSession = new MediaSessionCompat(this, TAG);


        //Enable callbacks from MediaButtons and TransportControls
        mMediaSession.setFlags(
                MediaSessionCompat.FLAG_HANDLES_MEDIA_BUTTONS |
                        MediaSessionCompat.FLAG_HANDLES_TRANSPORT_CONTROLS);


        //Do not let MediaButtons restart the player when the app is not visible
        mMediaSession.setMediaButtonReceiver(null);


        //Set an initial Playback with ACTION_PLAY, so media buttons can start the player
        mStateBuilder = new PlaybackStateCompat.Builder()
                .setActions(
                        PlaybackStateCompat.ACTION_PLAY |
                                PlaybackStateCompat.ACTION_PAUSE |
                                PlaybackStateCompat.ACTION_SKIP_TO_PREVIOUS |
                                PlaybackStateCompat.ACTION_PLAY_PAUSE);


        mMediaSession.setPlaybackState(mStateBuilder.build());




        //MySessionCallback has methods that handle callbacks from a media controller
        mMediaSession.setCallback(new MySessionCallback());


        //Start the Media Session since the activity is active
        mMediaSession.setActive(true);
    }
```


#### 2. Create an inner class that extends `MediaSessionCompat.Callbacks`, and override the` onPlay()`, `onPause()`, and `onSkipToPrevious()` callbacks. Pass an instance of this class into the `MediaSession.setCallback()` method in the method you created in TODO 1.[[code][2]]

```java
    private class MySessionCallback extends MediaSessionCompat.Callback {
        @Override
        public void onPlay() {
            mExoPlayer.setPlayWhenReady(true);
        }


        @Override
        public void onPause() {
            mExoPlayer.setPlayWhenReady(false);
        }


        @Override
        public void onSkipToPrevious() {
            mExoPlayer.seekTo(0);
        }
    }
```


#### 3. When `ExoPlayer` is playing, update the `PlayBackState`.[[code][3]]

```java
            mStateBuilder.setState(PlaybackStateCompat.STATE_PLAYING,
                    mExoPlayer.getCurrentPosition(), 1f);
```


#### 4. When `ExoPlayer` is paused, update the `PlayBackState`.[[code][4]]

```java
            mStateBuilder.setState(PlaybackStateCompat.STATE_PAUSED,
                    mExoPlayer.getCurrentPosition(), 1f);
```


#### 5. When the activity is destroyed, set the `MediaSession` to inactive.[[code][5]]

```java
        mMediaSession.setActive(false);
```

## _Class Reference_
### [MediaSessionCompat](https://developer.android.com/reference/android/support/v4/media/session/MediaSessionCompat.html)
Allows interaction with media controllers, volume keys, media buttons, and transport controls.

|Return Type   |Method Name   |
|:--------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `void` | [setActive](https://developer.android.com/reference/android/support/v4/media/session/MediaSessionCompat.html#setActive(boolean))`(boolean active)` <br/>Sets if this session is currently active and ready to receive commands. |
| `void` | [setCallback](https://developer.android.com/reference/android/support/v4/media/session/MediaSessionCompat.html#setCallback(android.support.v4.media.session.MediaSessionCompat.Callback))`(`[MediaSessionCompat.Callback](https://developer.android.com/reference/android/support/v4/media/session/MediaSessionCompat.Callback.html) `callback)` <br/>Adds a callback to receive updates on for the MediaSession. |
| `void` | [setFlags](https://developer.android.com/reference/android/support/v4/media/session/MediaSessionCompat.html#setFlags(int))`(int flags)` <br/>Sets any flags for the session. |
| `void` | [setMediaButtonReceiver](https://developer.android.com/reference/android/support/v4/media/session/MediaSessionCompat.html#setMediaButtonReceiver(android.app.PendingIntent))`(`[PendingIntent](https://developer.android.com/reference/android/app/PendingIntent.html) `mbr)` <br/>Sets a pending intent for your media button receiver to allow restarting playback after the session has been stopped. |
| `void` | [setPlaybackState](https://developer.android.com/reference/android/support/v4/media/session/MediaSessionCompat.html#setPlaybackState(android.support.v4.media.session.PlaybackStateCompat))`(`[PlaybackStateCompat](https://developer.android.com/reference/android/support/v4/media/session/PlaybackStateCompat.html) `state)` <br/>Updates the current playback state. |


### [PlaybackStateCompat.Builder](https://developer.android.com/reference/android/support/v4/media/session/PlaybackStateCompat.Builder.html)
Builder for [PlaybackStateCompat](https://developer.android.com/reference/android/support/v4/media/session/PlaybackStateCompat.html) objects.

|Return Type   |Method Name   |
|:--------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [PlaybackStateCompat](https://developer.android.com/reference/android/support/v4/media/session/PlaybackStateCompat.html) | [build](https://developer.android.com/reference/android/support/v4/media/session/PlaybackStateCompat.Builder.html#build())`()` <br/>Creates the playback state object. |
| [PlaybackStateCompat.Builder](https://developer.android.com/reference/android/support/v4/media/session/PlaybackStateCompat.Builder.html) | [setActions](https://developer.android.com/reference/android/support/v4/media/session/PlaybackStateCompat.Builder.html#setActions(long))`(long capabilities)` <br/>Set the current capabilities available on this session. |
| [PlaybackStateCompat.Builder](https://developer.android.com/reference/android/support/v4/media/session/PlaybackStateCompat.Builder.html) | [setState](https://developer.android.com/reference/android/support/v4/media/session/PlaybackStateCompat.Builder.html#setState) `(int state, long position, float playbackSpeed)` <br/>Set the current state of playback. |


### [MediaSessionCompat.Callback](https://developer.android.com/reference/android/media/session/MediaSession.Callback.html)
Receives media buttons, transport controls, and commands from controllers and the system. A callback may be set using [setCallback(MediaSession.Callback)](https://developer.android.com/reference/android/media/session/MediaSession.html#setCallback(android.media.session.MediaSession.Callback)).

|Return Type   |Method Name   |
|:--------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `void` | [onPause](https://developer.android.com/reference/android/media/session/MediaSession.Callback.html#onPause())`()` <br/>Override to handle requests to pause playback. |
| `void` | [onPlay](https://developer.android.com/reference/android/media/session/MediaSession.Callback.html#onPlay())`()` <br/>Override to handle requests to begin playback. |
| `void` | [onSkipToPrevious](https://developer.android.com/reference/android/media/session/MediaSession.Callback.html#onSkipToPrevious())`()` <br/>Override to handle requests to skip to the previous media item. |




[1]: https://github.com/aaroncrutchfield/AdvancedAndroid_ClassicalMusicQuiz/blob/87ad73fc9a3c6fef464a5d4bd0b4eec9b9e2cbcf/app/src/main/java/com/example/android/classicalmusicquiz/QuizActivity.java#L130-L158
[2]: https://github.com/aaroncrutchfield/AdvancedAndroid_ClassicalMusicQuiz/blob/87ad73fc9a3c6fef464a5d4bd0b4eec9b9e2cbcf/app/src/main/java/com/example/android/classicalmusicquiz/QuizActivity.java#L349-L364
[3]: https://github.com/aaroncrutchfield/AdvancedAndroid_ClassicalMusicQuiz/blob/87ad73fc9a3c6fef464a5d4bd0b4eec9b9e2cbcf/app/src/main/java/com/example/android/classicalmusicquiz/QuizActivity.java#L328-L329
[4]: https://github.com/aaroncrutchfield/AdvancedAndroid_ClassicalMusicQuiz/blob/87ad73fc9a3c6fef464a5d4bd0b4eec9b9e2cbcf/app/src/main/java/com/example/android/classicalmusicquiz/QuizActivity.java#L332-L333
[5]: https://github.com/aaroncrutchfield/AdvancedAndroid_ClassicalMusicQuiz/blob/87ad73fc9a3c6fef464a5d4bd0b4eec9b9e2cbcf/app/src/main/java/com/example/android/classicalmusicquiz/QuizActivity.java#L306
