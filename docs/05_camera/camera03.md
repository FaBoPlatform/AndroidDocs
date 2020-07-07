# Cameraへのオーバーレイ

CameraにViewをOverlayしてみます。


MainActivity.java
```java
package utsunomiya.gclue.com.camerasample;

import android.app.Activity;
import android.os.Bundle;
import android.view.Menu;
import android.view.MenuItem;
import android.view.ViewGroup;
import android.view.Window;
import android.view.WindowManager;

public class MainActivity extends Activity {

    /** LOGCAT用のTAG. */
    private static final String TAG = "CAMERA_ACTIVITY";

    /** Camera用のSurfaceView. */
    private static CameraView mCameraView;

    /** OverlayするView. */
    private static MyView mView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        getWindow().addFlags(WindowManager.LayoutParams.FLAG_FULLSCREEN);
        requestWindowFeature(Window.FEATURE_NO_TITLE);

        mCameraView = new CameraView(this);
        setContentView(mCameraView);

        mView = new MyView(this);
        addContentView(mView, new ViewGroup.LayoutParams(ViewGroup.LayoutParams.WRAP_CONTENT, ViewGroup.LayoutParams.WRAP_CONTENT));
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

MyView.java
```java
package utsunomiya.gclue.com.camerasample;

/**
 * Created by sasakiakira on 15/10/30.
 */
import android.content.Context;
import android.graphics.Canvas;
import android.graphics.Paint;
import android.view.MotionEvent;
import android.view.View;

class MyView extends View {

    private int x;
    private int y;

    /**
     * コンストラクタ
     *
     * @param context
     */
    public MyView(Context context) {
        super(context);
        setFocusable(true);
    }

    /**
     * 描画処理
     */
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);

        // 描画するための線の色を設定.
        Paint mPaint = new Paint();
        mPaint.setStyle( Paint.Style.FILL );
        mPaint.setARGB( 255, 255, 255, 100 );

        // 長方形を描画.
        canvas.drawRect( x, y, x + 100, y + 100, mPaint );
    }

    /**
     * タッチイベント
     */
    public boolean onTouchEvent(MotionEvent event) {

        // X,Y座標の取得
        x = (int)event.getX();
        y = (int)event.getY();

        // 再描画の指示
        invalidate();

        return true;
    }
}
```

## 加速度の値をOverlay

MyView.java
```java
package utsunomiya.gclue.com.camerasample;

/**
 * Created by sasakiakira on 15/10/30.
 */
import android.content.Context;
import android.graphics.Canvas;
import android.graphics.Paint;
import android.view.MotionEvent;
import android.view.View;
import android.hardware.Sensor;
import android.hardware.SensorEvent;
import android.hardware.SensorEventListener;
import android.hardware.SensorManager;

import java.util.List;

class MyView extends View implements SensorEventListener {

    /** タッチしたX座標. */
    private int x;
    /** タッチしたY座標. */
    private int y;
    /** SensorManagerのインスタンス. */
    private SensorManager mSensorManager = null;
    /** Log用のTag. */
    private final static String TAG = "SNESOR";

    /** 加速度X. */
    private Float acceleroX;
    /** 加速度Y. */
    private Float acceleroY;
    /** 加速度Z. */
    private Float acceleroZ;

    /**
     * コンストラクタ.
     *
     * @param context
     */
    public MyView(Context context) {
        super(context);
        setFocusable(true);

        // SensorManager
        mSensorManager = (SensorManager) context.getSystemService(Context.SENSOR_SERVICE);

        // 加速度の取得とリスナーへの登録
        List<Sensor> mSensors = mSensorManager.getSensorList(Sensor.TYPE_ACCELEROMETER);
        if (mSensors.size() > 0) {
            Sensor sensor = mSensors.get(0);
            mSensorManager.registerListener(this, sensor, SensorManager.SENSOR_DELAY_FASTEST);
        }
    }

    /**
     * 描画処理.
     */
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);

        // 描画するための線の色を設定.
        Paint mPaint = new Paint();
        mPaint.setStyle( Paint.Style.FILL );
        mPaint.setARGB( 255, 255, 255, 100 );
        mPaint.setTextSize(50);

        // 長方形を描画.
        canvas.drawRect( x, y, x + 100, y + 100, mPaint );

        // 文字を描画.
        canvas.drawText("acceleroX:" + acceleroX, 150, 50, mPaint);
        canvas.drawText("acceleroY:" + acceleroY, 150, 110, mPaint);
        canvas.drawText("acceleroZ:" + acceleroZ, 150, 170, mPaint);
    }

    /**
     * タッチイベント.
     */
    public boolean onTouchEvent(MotionEvent event) {

        // X,Y座標の取得.
        x = (int)event.getX();
        y = (int)event.getY();

        // 再描画の指示.
        invalidate();

        return true;
    }

    @Override
    public void onSensorChanged(SensorEvent event) {
        if (event.sensor.getType() == Sensor.TYPE_ACCELEROMETER) {
            acceleroX = event.values[0];
            acceleroY = event.values[1];
            acceleroZ = event.values[2];

            // 再描画の指示.
            invalidate();
        }
    }

    @Override
    public void onAccuracyChanged(Sensor sensor, int accuracy) {

    }
}
```

## 9軸を表示する

MyView.java
```java
package utsunomiya.gclue.com.camerasample;

/**
 * Created by sasakiakira on 15/10/30.
 */
import android.content.Context;
import android.graphics.Canvas;
import android.graphics.Paint;
import android.view.MotionEvent;
import android.view.View;
import android.hardware.Sensor;
import android.hardware.SensorEvent;
import android.hardware.SensorEventListener;
import android.hardware.SensorManager;

import java.util.List;

class MyView extends View implements SensorEventListener {

    /** タッチしたX座標. */
    private int x;
    /** タッチしたY座標. */
    private int y;
    /** SensorManagerのインスタンス. */
    private SensorManager mSensorManager = null;
    /** Log用のTag. */
    private final static String TAG = "SNESOR";

    /** 加速度X. */
    private Float acceleroX;
    /** 加速度Y. */
    private Float acceleroY;
    /** 加速度Z. */
    private Float acceleroZ;

    /** ジャイロX. */
    private Float gyroX;
    /** ジャイロY. */
    private Float gyroY;
    /** ジャイロZ. */
    private Float gyroZ;

    /** 加速度X. */
    private Float vectX;
    /** 加速度Y. */
    private Float vectY;
    /** 加速度Z. */
    private Float vectZ;

    /**
     * コンストラクタ.
     *
     * @param context
     */
    public MyView(Context context) {
        super(context);
        setFocusable(true);

        // SensorManager
        mSensorManager = (SensorManager) context.getSystemService(Context.SENSOR_SERVICE);

        // 加速度の取得とリスナーへの登録
        List<Sensor> mSensors = mSensorManager.getSensorList(Sensor.TYPE_ALL);
        for (int i = 0; i < mSensors.size(); i++) {
            Sensor sensor = mSensors.get(i);
            mSensorManager.registerListener(this, sensor, SensorManager.SENSOR_DELAY_NORMAL);
        }
    }

    /**
     * 描画処理.
     */
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);

        // 描画するための線の色を設定.
        Paint mPaint = new Paint();
        mPaint.setStyle( Paint.Style.FILL );
        mPaint.setARGB( 255, 255, 255, 100 );
        mPaint.setTextSize(50);

        // 長方形を描画.
        canvas.drawRect(x, y, x + 100, y + 100, mPaint);

        // 文字を描画.
        canvas.drawText("acceleroX:" + acceleroX, 150, 50, mPaint);
        canvas.drawText("acceleroY:" + acceleroY, 150, 110, mPaint);
        canvas.drawText("acceleroZ:" + acceleroZ, 150, 170, mPaint);

        // 文字を描画.
        canvas.drawText("gyroX:" + gyroX, 150, 250, mPaint);
        canvas.drawText("gyroY:" + gyroY, 150, 310, mPaint);
        canvas.drawText("gyroZ:" + gyroZ, 150, 370, mPaint);

        // 文字を描画.
        canvas.drawText("vectX:" + vectX, 150, 450, mPaint);
        canvas.drawText("vectY:" + vectY, 150, 510, mPaint);
        canvas.drawText("vectZ:" + vectZ, 150, 570, mPaint);
    }

    /**
     * タッチイベント.
     */
    public boolean onTouchEvent(MotionEvent event) {

        // X,Y座標の取得.
        x = (int)event.getX();
        y = (int)event.getY();

        // 再描画の指示.
        invalidate();

        return true;
    }

    @Override
    public void onSensorChanged(SensorEvent event) {
        if (event.sensor.getType() == Sensor.TYPE_ACCELEROMETER) {
            acceleroX = event.values[0];
            acceleroY = event.values[1];
            acceleroZ = event.values[2];

            // 再描画の指示.
            invalidate();
        }
        else if (event.sensor.getType() == Sensor.TYPE_GYROSCOPE) {
            gyroX = event.values[0];
            gyroY = event.values[1];
            gyroZ = event.values[2];

            // 再描画の指示.
            invalidate();
        }
        else if (event.sensor.getType() == Sensor.TYPE_ROTATION_VECTOR) {
            vectX = event.values[0];
            vectY = event.values[1];
            vectZ = event.values[2];

            // 再描画の指示.
            invalidate();
        }
    }

    @Override
    public void onAccuracyChanged(Sensor sensor, int accuracy) {

    }
}
```
