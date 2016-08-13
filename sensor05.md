# 照度センサ

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
    Sensor sensor;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // SensorManager
        mSensorManager = (SensorManager)getSystemService(SENSOR_SERVICE);

        // ローテーションの取得とリスナーへの登録
        List<Sensor> Sensors = mSensorManager.getSensorList(Sensor.TYPE_LIGHT);

        if (Sensors.size() > 0) {
            Sensor sensor = Sensors.get(0);
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
        Log.i(TAG, "SensorChanged():");

       if (event.sensor.getType() == Sensor.TYPE_LIGHT) {
           Log.i(TAG, "Light Level:" + event.values[0]);
        }
    }

    @Override
    public void onAccuracyChanged(Sensor sensor, int accuracy) {
        Log.i(TAG, "onAccuracyChanged()");
    }
}

```