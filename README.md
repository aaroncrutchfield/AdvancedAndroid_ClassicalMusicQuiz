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
5. Create a method called initializePlayer() that takes a Uri as an argument and call it here, passing in the Sample URI. [[code][5]]
6. Instantiate a SimpleExoPlayer object using DefaultTrackSelector and DefaultLoadControl. [[code][6]]
7. Prepare the MediaSource using DefaultDataSourceFactory and DefaultExtractorsFactory, as well as the Sample URI you passed in. [[code][7]]
8. Prepare the ExoPlayer with the MediaSource, start playing the sample and set the SimpleExoPlayer to the SimpleExoPlayerView. [[code][8]]
9. Stop the playback when you go to the next question. [[code][9]]
10. Change the default artwork in the SimpleExoPlayerView to show the picture of the composer, when the user has answered the question. [[code][10]]
11. Override onDestroy() to stop and release the player when the Activity is destroyed. [[code][11]]

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
