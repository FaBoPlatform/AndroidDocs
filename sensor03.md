# 回転度

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

        // ローテーションの取得とリスナーへの登録
        List<Sensor> rotationSensors = mSensorManager.getSensorList(Sensor.TYPE_ROTATION_VECTOR);
        Log.d(TAG,"Count:"+rotationSensors.size());
        if (rotationSensors.size() > 0) {
            Sensor sensor = rotationSensors.get(0);
            mSensorManager.registerListener(this, sensor, SensorManager.SENSOR_DELAY_NORMAL);
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

       if (event.sensor.getType() == Sensor.TYPE_ROTATION_VECTOR) {
            Log.i(TAG, "X-vec:" + event.values[0]);
            Log.i(TAG, "Y-vec:" + event.values[1]);
            Log.i(TAG, "Z-vec:" + event.values[2]);
        }
    }

    @Override
    public void onAccuracyChanged(Sensor sensor, int accuracy) {

    }
}

```

# Geometric系

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

        // ローテーションの取得とリスナーへの登録
        List<Sensor> rotationSensors = mSensorManager.getSensorList(Sensor.TYPE_GEOMAGNETIC_ROTATION_VECTOR);
        Log.d(TAG,"Count:"+rotationSensors.size());
        if (rotationSensors.size() > 0) {
            Sensor sensor = rotationSensors.get(0);
            mSensorManager.registerListener(this, sensor, SensorManager.SENSOR_DELAY_NORMAL);
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

       if (event.sensor.getType() == Sensor.TYPE_GEOMAGNETIC_ROTATION_VECTOR) {
            Log.i(TAG, "X-vec:" + event.values[0]);
            Log.i(TAG, "Y-vec:" + event.values[1]);
            Log.i(TAG, "Z-vec:" + event.values[2]);
        }
    }

    @Override
    public void onAccuracyChanged(Sensor sensor, int accuracy) {

    }
}

```

# Game用

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

        // ローテーションの取得とリスナーへの登録
        List<Sensor> rotationSensors = mSensorManager.getSensorList(Sensor.TYPE_GAME_ROTATION_VECTOR);
        Log.d(TAG,"Count:"+rotationSensors.size());
        if (rotationSensors.size() > 0) {
            Sensor sensor = rotationSensors.get(0);
            mSensorManager.registerListener(this, sensor, SensorManager.SENSOR_DELAY_GAME);
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

       if (event.sensor.getType() == Sensor.TYPE_GAME_ROTATION_VECTOR) {
            Log.i(TAG, "X-vec:" + event.values[0]);
            Log.i(TAG, "Y-vec:" + event.values[1]);
            Log.i(TAG, "Z-vec:" + event.values[2]);
        }
    }

    @Override
    public void onAccuracyChanged(Sensor sensor, int accuracy) {

    }
}

```