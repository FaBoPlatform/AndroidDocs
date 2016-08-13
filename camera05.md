# 端末の方角の計算


MainActivity.java
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

    /** 方位X. */
    private Float rotX;
    /** 方位Y. */
    private Float rotY;
    /** 方位Z. */
    private Float rotZ;

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
    /** 重力. */
    private float[] mGravity;
    /** 磁場. */
    private float[] mMagnetic;
    /** 重力フラグ. */
    private Boolean flagGravity = false;
    /** 磁場フラグ. */
    private Boolean flagMagnetic = false;

    /**
     * コンストラクタ.
     *
     * @param context
     */
    public MyView(Context context) {
        super(context);

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
        canvas.drawText("rotX:" + rotX, 150, 50, mPaint);
        canvas.drawText("rotY:" + rotY, 150, 110, mPaint);
        canvas.drawText("rotZ:" + rotZ, 150, 170, mPaint);

        // 文字を描画.
        canvas.drawText("lat:" + lat, 150, 650, mPaint);
        canvas.drawText("lon:" + lon, 150, 710, mPaint);
        canvas.drawText("distance:" + distance, 150, 770, mPaint);
        canvas.drawText("direction:" + direction, 150, 830, mPaint);
    }

    @Override
    public void onSensorChanged(SensorEvent event) {

        if (event.sensor.getType() == Sensor.TYPE_ACCELEROMETER) {
            mGravity = event.values.clone();
            flagGravity = true;
        }
        else if (event.sensor.getType() == Sensor.TYPE_MAGNETIC_FIELD) {
            mMagnetic = event.values.clone();
            flagMagnetic = true;
        }

        if (flagGravity && flagMagnetic) {
            flagGravity = false;
            flagMagnetic = false;

            // 角度を計算
            float[] matrixI = new float[9];
            float[] matrixR = new float[9];
            SensorManager.getRotationMatrix(matrixI, null, mGravity, mMagnetic);
            SensorManager.remapCoordinateSystem(matrixI, SensorManager.AXIS_X, SensorManager.AXIS_Z, matrixR);

            float[] martixRot = new float[3];
            SensorManager.getOrientation(matrixR, martixRot);
            for (int i = 0; i < martixRot.length; i++) {
                Double degrees = (martixRot[i] * 180) / Math.PI;
                martixRot[i] = degrees.floatValue();
            }

            rotX = martixRot[0];
            rotY = martixRot[1];
            rotZ = martixRot[2];
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