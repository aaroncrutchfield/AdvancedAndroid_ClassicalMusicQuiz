# activity_quiz.xml
1. Replace the Composer ImageView with a SimpleExoPlayerView that occupies the top half of your screen. [[code][1]]
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

# QuizActivity.java
2. Replace the ImageView with the SimpleExoPlayerView, and remove the method calls on the composerView. [[code][2]]
```java
        mExoPlayerView = (SimpleExoPlayerView) findViewById(R.id.pv_exo_player);
```

3. Replace the default artwork in the SimpleExoPlayerView with the question mark drawable. [[code][3]]
```java        
        // Load the image of the composer for the answer into the ImageView.
        Bitmap defaulatBitmap = BitmapFactory.decodeResource(getResources(), R.drawable.question_mark);
        mExoPlayerView.setDefaultArtwork(defaulatBitmap);
```

4. Create a Sample object using the Sample.getSampleByID() method and passing in mAnswerSampleID. [[code][4]]
```java
        Sample sample = Sample.getSampleByID(this, mAnswerSampleID);
```

5. Create a method called initializePlayer() that takes a Uri as an argument and call it here, passing in the Sample URI. [[code][5]]
```java
        initializePlayer(Uri.parse(sample.getUri()));
```

6. Instantiate a SimpleExoPlayer object using DefaultTrackSelector and DefaultLoadControl. [[code][6]]
```java
            TrackSelector defaultTrackSelector = new DefaultTrackSelector();
            LoadControl defaultLoadControl = new DefaultLoadControl();
            mExoPlayer = ExoPlayerFactory.newSimpleInstance(this, defaultTrackSelector, defaultLoadControl);
            mExoPlayerView.setPlayer(mExoPlayer);
```

7. Prepare the MediaSource using DefaultDataSourceFactory and DefaultExtractorsFactory, as well as the Sample URI you passed in. [[code][7]]
```java
            String userAgent = Util.getUserAgent(this, "ClassicalMusicQuiz");
            MediaSource mediaSource = new ExtractorMediaSource(
                    uri,
                    new DefaultDataSourceFactory(this, userAgent),
                    new DefaultExtractorsFactory(),
                    null,
                    null);
```

8. Prepare the ExoPlayer with the MediaSource, start playing the sample and set the SimpleExoPlayer to the SimpleExoPlayerView. [[code][8]]
```java
            mExoPlayer.prepare(mediaSource);
            mExoPlayer.setPlayWhenReady(true);
```

9. Stop the playback when you go to the next question. [[code][9]]
```java
            mExoPlayer.stop();
```

10. Change the default artwork in the SimpleExoPlayerView to show the picture of the composer, when the user has answered the question. [[code][10]]
```java
        mExoPlayerView.setDefaultArtwork(Sample.getComposerArtBySampleID(this, mAnswerSampleID));
```

11. Override onDestroy() to stop and release the player when the Activity is destroyed. [[code][11]]
```java
    @Override
    protected void onDestroy() {
        super.onDestroy();
        mExoPlayer.stop();
        mExoPlayer.release();
        mExoPlayer = null;
    }
```

# Class References
### [SimpleExoPlayerView](http://google.github.io/ExoPlayer/doc/reference/com/google/android/exoplayer2/ui/SimpleExoPlayerView.html)
A high level view for SimpleExoPlayer media playbacks. It displays video, subtitles and album art during playback, and displays playback controls using a PlaybackControlView.

### [SimpleExoPlayer](http://google.github.io/ExoPlayer/doc/reference/com/google/android/exoplayer2/SimpleExoPlayer.html)

##### Methods
<table class="memberSummary" border="0" cellpadding="3" cellspacing="0" summary="Method Summary table, listing methods, and an explanation" style="width: 769px; border-left: 1px solid rgb(238, 238, 238); border-right: 1px solid rgb(238, 238, 238); border-bottom: 1px solid rgb(238, 238, 238); padding: 0px; color: rgb(53, 56, 51); font-family: &quot;DejaVu Sans&quot;, Arial, Helvetica, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: left; text-indent: 0px; text-transform: none; white-space: normal; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; text-decoration-style: initial; text-decoration-color: initial;"><tbody><tr id="i7" class="rowColor" style="background-color: rgb(238, 238, 239);"><td class="colFirst" style="text-align: left; padding: 8px 0px 3px 10px; width: 133px; vertical-align: top; white-space: nowrap; font-size: 13px;"><code style="font-family: &quot;DejaVu Sans Mono&quot;, monospace; font-size: 14px; padding-top: 4px; margin-top: 8px; line-height: 1.4em;"><a href="http://google.github.io/ExoPlayer/doc/reference/com/google/android/exoplayer2/SimpleExoPlayer.html" title="class in com.google.android.exoplayer2" style="text-decoration: none; color: rgb(74, 103, 130); font-weight: bold;">SimpleExoPlayer</a></code></td><td class="colLast" style="text-align: left; padding: 8px 0px 3px 10px; width: 616px; vertical-align: top; font-size: 13px;"><code style="font-family: &quot;DejaVu Sans Mono&quot;, monospace; font-size: 14px; padding-top: 4px; margin-top: 8px; line-height: 1.4em;"><span class="memberNameLink" style="font-weight: bold;"><a href="http://google.github.io/ExoPlayer/doc/reference/com/google/android/exoplayer2/ui/SimpleExoPlayerView.html#getPlayer--" style="text-decoration: none; color: rgb(74, 103, 130); padding-bottom: 3px; font-weight: bold;">getPlayer</a></span>()</code><div class="block" style="display: block; margin: 3px 10px 2px 0px; color: rgb(71, 71, 71); font-size: 14px; font-family: &quot;DejaVu Serif&quot;, Georgia, &quot;Times New Roman&quot;, Times, serif; padding-top: 0px;">Returns the player currently set on this view, or null if no player is set</div></td></tr></tbody>
<tbody><tr id="i22" class="altColor" style="background-color: rgb(255, 255, 255);"><td class="colFirst" style="text-align: left; padding: 8px 0px 3px 10px; width: 133px; vertical-align: top; white-space: nowrap; font-size: 13px;"><code style="font-family: &quot;DejaVu Sans Mono&quot;, monospace; font-size: 14px; padding-top: 4px; margin-top: 8px; line-height: 1.4em;">void</code></td><td class="colLast" style="text-align: left; padding: 8px 0px 3px 10px; width: 616px; vertical-align: top; font-size: 13px;"><code style="font-family: &quot;DejaVu Sans Mono&quot;, monospace; font-size: 14px; padding-top: 4px; margin-top: 8px; line-height: 1.4em;"><span class="memberNameLink" style="font-weight: bold;"><a href="http://google.github.io/ExoPlayer/doc/reference/com/google/android/exoplayer2/ui/SimpleExoPlayerView.html#setPlayer-com.google.android.exoplayer2.SimpleExoPlayer-" style="text-decoration: none; color: rgb(74, 103, 130); padding-bottom: 3px; font-weight: bold;">setPlayer</a></span>(<a href="http://google.github.io/ExoPlayer/doc/reference/com/google/android/exoplayer2/SimpleExoPlayer.html" title="class in com.google.android.exoplayer2" style="text-decoration: none; color: rgb(74, 103, 130); padding-bottom: 3px; font-weight: bold;">SimpleExoPlayer</a> player)</code><div class="block" style="display: block; margin: 3px 10px 2px 0px; color: rgb(71, 71, 71); font-size: 14px; font-family: &quot;DejaVu Serif&quot;, Georgia, &quot;Times New Roman&quot;, Times, serif; padding-top: 0px;">Set the<span> </span><a href="http://google.github.io/ExoPlayer/doc/reference/com/google/android/exoplayer2/SimpleExoPlayer.html" title="class in com.google.android.exoplayer2" style="text-decoration: none; color: rgb(74, 103, 130); padding-bottom: 3px; font-weight: bold;"><code style="font-family: &quot;DejaVu Sans Mono&quot;, monospace; font-size: 14px; padding-top: 4px; margin-top: 8px; line-height: 1.4em;">SimpleExoPlayer</code></a><span> </span>to use.</div></td></tr></tbody>




</table>



# Screenshots
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