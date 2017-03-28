# Android UniversalVideoView

[中文版说明请点击这里](http://my.oschina.net/u/1403288/blog/522278)

UniversalVideoView is a Android widget helps playing video easier, which is similar with the Android system native `VideoView`,
but providing more customization feature: fitXY  or keeping aspect ratio fullscreen videoView, auto switch to fullscreen on landscape mode, customised control UI...

![Sample Screenshot 1](./screenshot/screen1.png)
![Sample Screenshot 2](./screenshot/screen2.png)

# Usage

*For a working implementation of this project see the sample app.*

  1. add library dependency to your `build.gradle` file.
```groovy
            dependencies {
                compile 'com.linsea:universalvideoview:1.1.0@aar'
            }
```
  2. Include the `UniversalVideoView` and  `UniversalMediaController` widget in your layout. This should usually be placed
     in the same parent `ViewGroup`, which makes sense when in full screen state.
```xml
            <FrameLayout
                android:id="@+id/video_layout"
                android:layout_width="fill_parent"
                android:layout_height="200dp"
                android:background="@android:color/black">

                <com.universalvideoview.UniversalVideoView
                    android:id="@+id/videoView"
                    android:layout_width="fill_parent"
                    android:layout_height="fill_parent"
                    android:layout_gravity="center"
                    app:uvv_autoRotation="true"
                    app:uvv_fitXY="false" />

                <com.universalvideoview.UniversalMediaController
                    android:id="@+id/media_controller"
                    android:layout_width="fill_parent"
                    android:layout_height="fill_parent"
                    app:uvv_scalable="true" />

            </FrameLayout>
```

  3. In your `onCreate` method, set the `UniversalMediaController` to the `UniversalVideoView` and implements the `UniversalVideoView.VideoViewCallback` Callback.
```java
            View mBottomLayout;
            View mVideoLayout;
            UniversalVideoView mVideoView;
            UniversalMediaController mMediaController;

            mVideoView = (UniversalVideoView) findViewById(R.id.videoView);
            mMediaController = (UniversalMediaController) findViewById(R.id.media_controller);
            mVideoView.setMediaController(mMediaController);

            mVideoView.setVideoViewCallback(new UniversalVideoView.VideoViewCallback() {
                @Override
                public void onScaleChange(boolean isFullscreen) {
                    this.isFullscreen = isFullscreen;
                    if (isFullscreen) {
                        ViewGroup.LayoutParams layoutParams = mVideoLayout.getLayoutParams();
                        layoutParams.width = ViewGroup.LayoutParams.MATCH_PARENT;
                        layoutParams.height = ViewGroup.LayoutParams.MATCH_PARENT;
                        mVideoLayout.setLayoutParams(layoutParams);
                        //GONE the unconcerned views to leave room for video and controller
                        mBottomLayout.setVisibility(View.GONE);
                    } else {
                        ViewGroup.LayoutParams layoutParams = mVideoLayout.getLayoutParams();
                        layoutParams.width = ViewGroup.LayoutParams.MATCH_PARENT;
                        layoutParams.height = this.cachedHeight;
                        mVideoLayout.setLayoutParams(layoutParams);
                        mBottomLayout.setVisibility(View.VISIBLE);
                    }
                }

                @Override
                public void onPause(MediaPlayer mediaPlayer) { // Video pause
                    Log.d(TAG, "onPause UniversalVideoView callback");
                }

                @Override
                public void onStart(MediaPlayer mediaPlayer) { // Video start/resume to play
                    Log.d(TAG, "onStart UniversalVideoView callback");
                }

                @Override
                public void onBufferingStart(MediaPlayer mediaPlayer) {// steam start loading
                    Log.d(TAG, "onBufferingStart UniversalVideoView callback");
                }

                @Override
                public void onBufferingEnd(MediaPlayer mediaPlayer) {// steam end loading
                    Log.d(TAG, "onBufferingEnd UniversalVideoView callback");
                }

            });
```

## Note

  * Support Android Gingerbread V2.3(API Level 9 and above).
  * UniversalVideoView does not retain its full state when going into the background.
    You should save or restore the state and take care of the [Activity Lifecycle](http://developer.android.com/intl/ko/guide/components/activities.html#Lifecycle).
  * You may need to set the `android:configChanges="orientation|keyboardHidden|screenSize"` for your `Activity` in `AndroidManifest.xml`
    to prevent the system from recreate the Activity while phone rotation.

# Customization
## `UniversalVideoView` attribute

 * `uvv_fitXY`, Video scale to fill the VideoView's dimension or keep Aspect Ratio (default) likes Android framework VideoView.
 * `uvv_autoRotation`, auto switch to landscape(fullscreen) or portrait mode according to the orientation sensor.
 
## `UniversalMediaController` attribute
 * `uvv_scalable`, show or hide the scale button. if you will not play the video in fullscreen.

# TODO
 * Brightness control on `UniversalMediaController`.
 * Volume Control on `UniversalMediaController`.

# License

    Copyright 2015 The UniversalVideoView author <dictfb@gmail.com>

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
