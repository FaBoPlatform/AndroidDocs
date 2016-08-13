# Gyroscope

MainActivity.java
```java
package utsunomiya.gclue.com.sensorsample;

import android.app.Activity;
import android.hardware.Sensor;
import android.hardware.SensorEvent;
import android.hardware.SensorEventListener;
import android.hardware.SensorManager;
import android.os.Bundle;
import android.util.Log;
import android.view.Menu;
import android.view.MenuItem;

import java.util.List;

public class MainActivity extends Activity implements SensorEventListener {

    /** SensorManagerのインスタンス. */
    private SensorManager mSensorManager = null;

    /** Log用のTag. */
    private final static String TAG = "SNESOR";

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // SensorManager
        mSensorManager = (SensorManager)getSystemService(SENSOR_SERVICE);

        // 加速度の取得とリスナーへの登録
        List<Sensor> axisSensors = mSensorManager.getSensorList(Sensor.TYPE_ACCELEROMETER);
        if (axisSensors.size() > 0) {
            Sensor sensor = axisSensors.get(0);
            mSensorManager.registerListener(this, sensor, SensorManager.SENSOR_DELAY_FASTEST);
        }

        // ジャイロの取得とリスナーへの登録
        List<Sensor> gyroSensors = mSensorManager.getSensorList(Sensor.TYPE_GYROSCOPE);
        if (gyroSensors.size() > 0) {
            Sensor sensor = gyroSensors.get(0);
            mSensorManager.registerListener(this, sensor, SensorManager.SENSOR_DELAY_FASTEST);
        }
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

    @Override
    public void onSensorChanged(SensorEvent event) {
        Log.i(TAG, "SensorChanged()");

        if (event.sensor.getType() == Sensor.TYPE_ACCELEROMETER) {
            Log.i(TAG, "X-axis (gravitational force)." + event.values[0]);
            Log.i(TAG, "Y-axis (gravitational force)." + event.values[1]);
            Log.i(TAG, "Z-axis (gravitational force)." + event.values[2]);
        }
        else if (event.sensor.getType() == Sensor.TYPE_GYROSCOPE) {
            Log.i(TAG, "yaw." + event.values[0]);
            Log.i(TAG, "raw." + event.values[1]);
            Log.i(TAG, "pitch." + event.values[2]);
        }
    }

    @Override
    public void onAccuracyChanged(Sensor sensor, int accuracy) {

    }
}

```

# 搭載Chip

| 端末 | Chip名 |
| -- | -- |
| Nexus7 | [MPU-6050](http://store.invensense.com/datasheets/invensense/MPU-6050_DataSheet_V3%204.pdf) |

# Reference

* [Gyroscopeの原理](https://www.youtube.com/watch?v=cquvA_IpEsA)
* [Gryoscope](https://www.youtube.com/watch?v=zwe6LEYF0j8)
* [MEMS](https://www.youtube.com/watch?v=CNmk-SeM0ZI)
