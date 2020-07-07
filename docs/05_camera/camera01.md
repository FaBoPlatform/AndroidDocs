# Camera用の土台の作成

## MainActivity.java
```java
package utsunomiya.gclue.com.camerasample;

import android.app.Activity;
import android.os.Bundle;
import android.view.Menu;
import android.view.MenuItem;
import android.view.Window;
import android.view.WindowManager;

public class MainActivity extends Activity {

    /** LOGCAT用のTAG. */
    private static final String TAG = "CAMERA_ACTIVITY";

    /** Camera用のSurfaceView. */
    private static CameraView mCameraView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        getWindow().addFlags(WindowManager.LayoutParams.FLAG_FULLSCREEN);
        requestWindowFeature(Window.FEATURE_NO_TITLE);

        mCameraView = new CameraView(this);
        setContentView(mCameraView);
    }

    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        // Inflate the menu; this adds items to the action bar if it is present.
        getMenuInflater().inflate(R.menu.menu_main, menu);
        return true;
    }

    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        // Handle action bar item clicks here. The action bar will
        // automatically handle clicks on the Home/Up button, so long
        // as you specify a parent activity in AndroidManifest.xml.
        int id = item.getItemId();

        //noinspection SimplifiableIfStatement
        if (id == R.id.action_settings) {
            return true;
        }

        return super.onOptionsItemSelected(item);
    }
}
```

Titleバーを廃止し、フルスクリーンにする

```
    getWindow().addFlags(WindowManager.LayoutParams.FLAG_FULLSCREEN);
    requestWindowFeature(Window.FEATURE_NO_TITLE);
```
## CameraView.java
```java
package utsunomiya.gclue.com.camerasample;

import android.content.Context;
import android.util.Log;
import android.view.SurfaceHolder;
import android.view.SurfaceView;

class CameraView extends SurfaceView implements SurfaceHolder.Callback {

    /** LOGCAT用のTAG. */
    private static final String TAG = "CAMERA_VIEW";

    CameraView(Context context) {
        super(context);

        getHolder().addCallback(this);
    }

    @Override
    protected void onLayout(boolean changed, int l, int t, int r, int b) {
        Log.d(TAG, "onLayout l:"+l+"t:"+t+"r:"+r+"b:"+b);
    }

    @Override
    public void surfaceCreated(SurfaceHolder holder) {
        Log.d(TAG, "surfaceCreated");
    }

    @Override
    public void surfaceChanged(SurfaceHolder holder, int format, int width, int height) {
        Log.d(TAG, "surfaceChanged format:"+format+"width:"+width+"height:"+height);
    }

    @Override
    public void surfaceDestroyed(SurfaceHolder holder) {
        Log.d(TAG, "surfaceDestroyed");
    }
}
```

SurfaceHolder.Callbackを実装する事で、SurfaceViewが生成された時、破棄された時、状態がかわった時に処理を追加する事が可能になります。

| メソッド名 | 概要 |
| -- | -- |
| surfaceChanged() | SurfaceViewに変化があった際に呼ばれる |
| surfaceCreated() | SurfaceViewが生成された際に呼ばれる |
| surfaceDestroyed() | SurfaceViewが破棄された際に呼ばれる |



