# CameraとGPS

AndroidManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="utsunomiya.gclue.com.camerasample" >

    <uses-permission android:name="android.permission.CAMERA" />
    <uses-feature android:name="android.hardware.camera" />
    <uses-feature android:name="android.hardware.camera.autofocus" />
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:theme="@style/AppTheme" >
        <activity
            android:name=".MainActivity"
            android:screenOrientation="landscape"
            android:label="@string/app_name" >
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>

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
import android.location.Location;
import android.location.LocationManager;
import android.location.LocationListener;
import android.os.Bundle;
import android.util.Log;
import android.view.MotionEvent;
import android.view.View;
import android.hardware.Sensor;
import android.hardware.SensorEvent;
import android.hardware.SensorEventListener;
import android.hardware.SensorManager;

import java.util.List;

class MyView extends View implements SensorEventListener, LocationListener {

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

    /** GPS. */
    private LocationManager mLocationManager;
    /** 緯度. */
    private double lat;
    /** 経度. */
    private double lon;
    /** 距離. */
    private double distance;
    /** 方位. */
    private double direction;

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

        // GPSの取得.
        mLocationManager = (LocationManager)context.getSystemService(Context.LOCATION_SERVICE);
        mLocationManager.requestLocationUpdates(LocationManager.GPS_PROVIDER, 1000, 50, this);
    }

    /**
     * 描画処理.
     */
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);

        // 描画するための線の色を設定.
        Paint mPaint = new Paint();
        mPaint.setStyle(Paint.Style.FILL);
        mPaint.setARGB(255, 255, 255, 100);
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

        // 文字を描画.
        canvas.drawText("lat:" + lat, 150, 650, mPaint);
        canvas.drawText("lon:" + lon, 150, 710, mPaint);
        canvas.drawText("distance:" + distance, 150, 770, mPaint);
        canvas.drawText("direction:" + direction, 150, 830, mPaint);
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

    @Override
    public void onLocationChanged(Location location) {
        lat = location.getLatitude();
        lon = location.getLongitude();

        Location fujisanLocation = new Location("fujisan");
        fujisanLocation.setLatitude(35.360744);
        fujisanLocation.setLongitude(138.727810);

        distance = location.distanceTo(fujisanLocation);
        direction = location.bearingTo(fujisanLocation);
    }

    @Override
    public void onStatusChanged(String provider, int status, Bundle extras) {

    }

    @Override
    public void onProviderEnabled(String provider) {

    }

    @Override
    public void onProviderDisabled(String provider) {

    }
}
```