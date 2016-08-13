# TextureViewの作成


Camera2では、ベースのViewは、TextureViewを用います。
まず、TextureViewの土台を作りイベントリスナーを登録します。

TextureViewに、SurfaceTextureリスナーを登録すると、onSurfaceTextureAvailable,onSurfaceTextureSizeChanged,onSurfaceTextureDestroyed,onSurfaceTextureUpdated にイベントが発生した歳に、イベントが飛んでくるようになります。

| メソッド名 | タイミング |
| -- | -- |
| onSurfaceTextureAvailable | SurfaceTextureが利用可能になった時 |
| onSurfaceTextureSizeChanged | SurfaceTextureのサイズが変更された時 |
| onSurfaceTextureDestroyed | SurfaceTextureが破棄された時 |
| onSurfaceTextureUpdated | SurfaceTextureが更新された時|


MainActivity.java
```Java
package utsunomiya.gclue.com.camerasample;

import android.app.Activity;
import android.graphics.SurfaceTexture;
import android.os.Bundle;
import android.util.Log;
import android.view.TextureView;

public class MainActivity extends Activity implements TextureView.SurfaceTextureListener {

    /** TextureView */
    private TextureView mTextureView;
    /** SurfaceTexture */
    private SurfaceTexture mSurfaceTexture;
    /** Log用のTag */
    private static final String TAG = "CAMERA";


    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // TextureViewをactivity_mainから取り込み
        mTextureView = (TextureView) findViewById(R.id.camera_texture_view);
        // TextureViewのリスターを設定
        mTextureView.setSurfaceTextureListener(this);
        // SurfaceTextureのインスタンスを取得
        mSurfaceTexture = mTextureView.getSurfaceTexture();
    }

    @Override
    public void onSurfaceTextureAvailable(SurfaceTexture surface, int width, int height) {
        Log.i(TAG, "onSurfaceTextureAvailable");
    }

    @Override
    public void onSurfaceTextureSizeChanged(SurfaceTexture surface, int width, int height) {
        Log.i(TAG, "onSurfaceTextureSizeChanged");

    }

    @Override
    public boolean onSurfaceTextureDestroyed(SurfaceTexture surface) {
        Log.i(TAG, "onSurfaceTextureDestroyed");

        return false;
    }

    @Override
    public void onSurfaceTextureUpdated(SurfaceTexture surface) {
        Log.i(TAG, "onSurfaceTextureUpdated");

    }
}
```
activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical" >


        <FrameLayout
            android:layout_width="match_parent"
            android:layout_height="match_parent">

            <TextureView
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:id="@+id/camera_texture_view" />

        </FrameLayout>

</LinearLayout>
```

AndroidManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="utsunomiya.gclue.com.camerasample" >

    <uses-permission android:name="android.permission.CAMERA" />
    <uses-feature android:name="android.hardware.camera" />
    <uses-feature android:name="android.hardware.camera.autofocus" />

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:theme="@style/AppTheme" >
        <activity
            android:name=".MainActivity"
            android:label="@string/app_name" >
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>

```
