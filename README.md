
# Exercise 1 - Add SimpleExoPlayerView
## *activity_quiz.xml*
#### 1. Replace the Composer `ImageView` with a `SimpleExoPlayerView` that occupies the top half of your screen. [[code][1]]
```xml
    <com.google.android.exoplayer2.ui.SimpleExoPlayerView
        android:id="@+id/pv_exo_player"
        android:layout_width="0dp"
        android:layout_height="0dp"
        app:layout_constraintBottom_toTopOf="@+id/horizontalHalf"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />
```

## *QuizActivity.java*
#### 2. Replace the `ImageView` with the `SimpleExoPlayerView`, and remove the method calls on the composerView. [[code][2]]
```java
        mExoPlayerView = (SimpleExoPlayerView) findViewById(R.id.pv_exo_player);
```

#### 3. Replace the default artwork in the `SimpleExoPlayerView` with the question mark drawable. [[code][3]]
```java        
        // Load the image of the composer for the answer into the ImageView.
        Bitmap defaulatBitmap = BitmapFactory.decodeResource(getResources(), R.drawable.question_mark);
        mExoPlayerView.setDefaultArtwork(defaulatBitmap);
```

#### 4. Create a `Sample` object using the `Sample.getSampleByID()` method and passing in `mAnswerSampleID`. [[code][4]]
```java
        Sample sample = Sample.getSampleByID(this, mAnswerSampleID);
```

#### 5. Create a method called `initializePlayer()` that takes a `Uri` as an argument and call it here, passing in the Sample `URI`. [[code][5]]
```java
        initializePlayer(Uri.parse(sample.getUri()));
```

#### 6. Instantiate a `SimpleExoPlayer` object using `DefaultTrackSelector` and `DefaultLoadControl`. [[code][6]]
```java
            TrackSelector defaultTrackSelector = new DefaultTrackSelector();
            LoadControl defaultLoadControl = new DefaultLoadControl();
            mExoPlayer = ExoPlayerFactory.newSimpleInstance(this, defaultTrackSelector, defaultLoadControl);
            mExoPlayerView.setPlayer(mExoPlayer);
```

#### 7. Prepare the `MediaSource` using `DefaultDataSourceFactory` and `DefaultExtractorsFactory`, as well as the Sample `URI` you passed in. [[code][7]]
```java
            String userAgent = Util.getUserAgent(this, "ClassicalMusicQuiz");
            MediaSource mediaSource = new ExtractorMediaSource(
                    uri,
                    new DefaultDataSourceFactory(this, userAgent),
                    new DefaultExtractorsFactory(),
                    null,
                    null);
```

#### 8. Prepare the `ExoPlayer` with the `MediaSource`, start playing the sample and set the `SimpleExoPlayer` to the `SimpleExoPlayerView`. [[code][8]]
```java
            mExoPlayer.prepare(mediaSource);
            mExoPlayer.setPlayWhenReady(true);
```

#### 9. Stop the playback when you go to the next question. [[code][9]]
```java
            mExoPlayer.stop();
```

#### 10. Change the default artwork in the `SimpleExoPlayerView` to show the picture of the composer, when the user has answered the question. [[code][10]]
```java
        mExoPlayerView.setDefaultArtwork(Sample.getComposerArtBySampleID(this, mAnswerSampleID));
```

#### 11. Override `onDestroy()` to stop and release the player when the `Activity` is destroyed. [[code][11]]
```java
    @Override
    protected void onDestroy() {
        super.onDestroy();
        mExoPlayer.stop();
        mExoPlayer.release();
        mExoPlayer = null;
    }
```

## *Class References*
### [SimpleExoPlayer](https://google.github.io/ExoPlayer/doc/reference/com/google/android/exoplayer2/SimpleExoPlayer.html)

An [ExoPlayer](https://google.github.io/ExoPlayer/doc/reference/com/google/android/exoplayer2/ExoPlayer.html) implementation that uses default [Renderer](https://google.github.io/ExoPlayer/doc/reference/com/google/android/exoplayer2/Renderer.html) components. Instances can be obtained from [ExoPlayerFactory](https://google.github.io/ExoPlayer/doc/reference/com/google/android/exoplayer2/ExoPlayerFactory.html).

|Return Type   |  Method Name |
|:------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| void | [prepare](https://google.github.io/ExoPlayer/doc/reference/com/google/android/exoplayer2/SimpleExoPlayer.html#prepare-com.google.android.exoplayer2.source.MediaSource-)([MediaSource](https://google.github.io/ExoPlayer/doc/reference/com/google/android/exoplayer2/source/MediaSource.html) mediaSource) <br> Prepares the player to play the provided [MediaSource](https://google.github.io/ExoPlayer/doc/reference/com/google/android/exoplayer2/source/MediaSource.html).                    |
| void | [release](https://google.github.io/ExoPlayer/doc/reference/com/google/android/exoplayer2/SimpleExoPlayer.html#release--)() <br>Releases the player.                                                                                                                                                                                                                                                                                                                                                |
| void | [setPlayWhenReady](https://google.github.io/ExoPlayer/doc/reference/com/google/android/exoplayer2/SimpleExoPlayer.html#setPlayWhenReady-boolean-)(boolean playWhenReady) <br>Sets whether playback should proceed when [Player.getPlaybackState()](https://google.github.io/ExoPlayer/doc/reference/com/google/android/exoplayer2/Player.html#getPlaybackState--) == [Player.STATE_READY](https://google.github.io/ExoPlayer/doc/reference/com/google/android/exoplayer2/Player.html#STATE_READY). |
| void | [stop](https://google.github.io/ExoPlayer/doc/reference/com/google/android/exoplayer2/SimpleExoPlayer.html#stop--)() <br>Stops playback.                                                                                                                                                                                                                                                                                                                                                           |

### [SimpleExoPlayerView](https://google.github.io/ExoPlayer/doc/reference/com/google/android/exoplayer2/ui/SimpleExoPlayerView.html)

A high level view for [SimpleExoPlayer](https://google.github.io/ExoPlayer/doc/reference/com/google/android/exoplayer2/SimpleExoPlayer.html) media playbacks. It displays video, subtitles and album art during playback, and displays playback controls using a [PlaybackControlView](https://google.github.io/ExoPlayer/doc/reference/com/google/android/exoplayer2/ui/PlaybackControlView.html).

|Return Type   |  Method Name|
|:------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| void | [setDefaultArtwork](https://google.github.io/ExoPlayer/doc/reference/com/google/android/exoplayer2/ui/SimpleExoPlayerView.html#setDefaultArtwork-android.graphics.Bitmap-)([Bitmap](https://developer.android.com/reference/android/graphics/Bitmap.html?is-external=true) defaultArtwork) <br>Sets the default artwork to display if useArtwork is true and no artwork is present in the media.                                                           |
| void | [setPlayer](https://google.github.io/ExoPlayer/doc/reference/com/google/android/exoplayer2/ui/SimpleExoPlayerView.html#setPlayer-com.google.android.exoplayer2.SimpleExoPlayer-)([SimpleExoPlayer](https://google.github.io/ExoPlayer/doc/reference/com/google/android/exoplayer2/SimpleExoPlayer.html) player)<br>Set the [SimpleExoPlayer](https://google.github.io/ExoPlayer/doc/reference/com/google/android/exoplayer2/SimpleExoPlayer.html) to use. |




## *Screenshots*
<img src="https://github.com/aaroncrutchfield/AdvancedAndroid_ClassicalMusicQuiz/blob/TMED.01-Exercise-AddExoPlayer/screenshots/screenshot1.png" width="300">
<img src="https://github.com/aaroncrutchfield/AdvancedAndroid_ClassicalMusicQuiz/blob/TMED.01-Exercise-AddExoPlayer/screenshots/screenshot2.png" width="300">
<img src="https://github.com/aaroncrutchfield/AdvancedAndroid_ClassicalMusicQuiz/blob/TMED.01-Exercise-AddExoPlayer/screenshots/screenshot3.png" width="300">

[1]: https://github.com/aaroncrutchfield/AdvancedAndroid_ClassicalMusicQuiz/blob/6c8379e4ecb05dbdf342beaabfb626b2dd864e1f/app/src/main/res/layout/activity_quiz.xml#L24-L32
[2]: https://github.com/aaroncrutchfield/AdvancedAndroid_ClassicalMusicQuiz/blob/fcded4c54561a6e9156ccbc1c524a6241a9f8027/app/src/main/java/com/example/android/classicalmusicquiz/QuizActivity.java#L68-L69
[3]: https://github.com/aaroncrutchfield/AdvancedAndroid_ClassicalMusicQuiz/blob/fcded4c54561a6e9156ccbc1c524a6241a9f8027/app/src/main/java/com/example/android/classicalmusicquiz/QuizActivity.java#L89-L92
[4]: https://github.com/aaroncrutchfield/AdvancedAndroid_ClassicalMusicQuiz/blob/fcded4c54561a6e9156ccbc1c524a6241a9f8027/app/src/main/java/com/example/android/classicalmusicquiz/QuizActivity.java#L103-L105
[5]: https://github.com/aaroncrutchfield/AdvancedAndroid_ClassicalMusicQuiz/blob/fcded4c54561a6e9156ccbc1c524a6241a9f8027/app/src/main/java/com/example/android/classicalmusicquiz/QuizActivity.java#L106-L107
[6]: https://github.com/aaroncrutchfield/AdvancedAndroid_ClassicalMusicQuiz/blob/fcded4c54561a6e9156ccbc1c524a6241a9f8027/app/src/main/java/com/example/android/classicalmusicquiz/QuizActivity.java#L113-L117
[7]: https://github.com/aaroncrutchfield/AdvancedAndroid_ClassicalMusicQuiz/blob/fcded4c54561a6e9156ccbc1c524a6241a9f8027/app/src/main/java/com/example/android/classicalmusicquiz/QuizActivity.java#L118-L125
[8]: https://github.com/aaroncrutchfield/AdvancedAndroid_ClassicalMusicQuiz/blob/fcded4c54561a6e9156ccbc1c524a6241a9f8027/app/src/main/java/com/example/android/classicalmusicquiz/QuizActivity.java#L126-L128
[9]: https://github.com/aaroncrutchfield/AdvancedAndroid_ClassicalMusicQuiz/blob/fcded4c54561a6e9156ccbc1c524a6241a9f8027/app/src/main/java/com/example/android/classicalmusicquiz/QuizActivity.java#L202-L203
[10]: https://github.com/aaroncrutchfield/AdvancedAndroid_ClassicalMusicQuiz/blob/fcded4c54561a6e9156ccbc1c524a6241a9f8027/app/src/main/java/com/example/android/classicalmusicquiz/QuizActivity.java#L217-L218
[11]: https://github.com/aaroncrutchfield/AdvancedAndroid_ClassicalMusicQuiz/blob/fcded4c54561a6e9156ccbc1c524a6241a9f8027/app/src/main/java/com/example/android/classicalmusicquiz/QuizActivity.java#L238-L245

[SimpleExoPlayer]: http://google.github.io/ExoPlayer/doc/reference/com/google/android/exoplayer2/ui/SimpleExoPlayerView.html
