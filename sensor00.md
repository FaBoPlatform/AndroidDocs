# Sensor

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

public class MainActivity extends Activity {

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

        List<Sensor> sensorList = mSensorManager.getSensorList(Sensor.TYPE_ALL);
        for (Sensor sensor : sensorList) {
            Log.d(TAG, "Sensor:" + sensor.getName());
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
}

```

# Nexus7
```
10-23 09:00:29.406    6104-6104/utsunomiya.gclue.com.sensorsample D/SNESOR﹕ Sensor:MPU6515 Accelerometer
10-23 09:00:29.406    6104-6104/utsunomiya.gclue.com.sensorsample D/SNESOR﹕ Sensor:AK8963 Magnetometer
10-23 09:00:29.406    6104-6104/utsunomiya.gclue.com.sensorsample D/SNESOR﹕ Sensor:AK8963 Magnetometer Uncalibrated
10-23 09:00:29.406    6104-6104/utsunomiya.gclue.com.sensorsample D/SNESOR﹕ Sensor:MPU6515 Gyroscope
10-23 09:00:29.406    6104-6104/utsunomiya.gclue.com.sensorsample D/SNESOR﹕ Sensor:MPU6515 Gyroscope Uncalibrated
10-23 09:00:29.406    6104-6104/utsunomiya.gclue.com.sensorsample D/SNESOR﹕ Sensor:APDS-9930/QPDS-T930 Proximity & Light
10-23 09:00:29.406    6104-6104/utsunomiya.gclue.com.sensorsample D/SNESOR﹕ Sensor:APDS-9930/QPDS-T930 Proximity & Light
10-23 09:00:29.406    6104-6104/utsunomiya.gclue.com.sensorsample D/SNESOR﹕ Sensor:BMP280 Barometer
10-23 09:00:29.406    6104-6104/utsunomiya.gclue.com.sensorsample D/SNESOR﹕ Sensor:Gravity
10-23 09:00:29.406    6104-6104/utsunomiya.gclue.com.sensorsample D/SNESOR﹕ Sensor:Linear Acceleration
10-23 09:00:29.406    6104-6104/utsunomiya.gclue.com.sensorsample D/SNESOR﹕ Sensor:Rotation Vector
10-23 09:00:29.406    6104-6104/utsunomiya.gclue.com.sensorsample D/SNESOR﹕ Sensor:Step Detector
10-23 09:00:29.406    6104-6104/utsunomiya.gclue.com.sensorsample D/SNESOR﹕ Sensor:Step Counter
10-23 09:00:29.406    6104-6104/utsunomiya.gclue.com.sensorsample D/SNESOR﹕ Sensor:Significant Motion Detector
10-23 09:00:29.406    6104-6104/utsunomiya.gclue.com.sensorsample D/SNESOR﹕ Sensor:Game Rotation Vector
10-23 09:00:29.406    6104-6104/utsunomiya.gclue.com.sensorsample D/SNESOR﹕ Sensor:GeoMagnetic Rotation Vector
10-23 09:00:29.406    6104-6104/utsunomiya.gclue.com.sensorsample D/SNESOR﹕ Sensor:Orientation
10-23 09:00:29.406    6104-6104/utsunomiya.gclue.com.sensorsample D/SNESOR﹕ Sensor:Tilt Detector
10-23 09:00:29.406    6104-6104/utsunomiya.gclue.com.sensorsample D/SNESOR﹕ Sensor:MPU6515 Game Rotation Vector Secondary
10-23 09:00:29.406    6104-6104/utsunomiya.gclue.com.sensorsample D/SNESOR﹕ Sensor:AMD
10-23 09:00:29.406    6104-6104/utsunomiya.gclue.com.sensorsample D/SNESOR﹕ Sensor:RMD
10-23 09:00:29.406    6104-6104/utsunomiya.gclue.com.sensorsample D/SNESOR﹕ Sensor:Basic Gestures
10-23 09:00:29.406    6104-6104/utsunomiya.gclue.com.sensorsample D/SNESOR﹕ Sensor:Tap
10-23 09:00:29.406    6104-6104/utsunomiya.gclue.com.sensorsample D/SNESOR﹕ Sensor:Facing
10-23 09:00:29.407    6104-6104/utsunomiya.gclue.com.sensorsample D/SNESOR﹕ Sensor:Tilt
10-23 09:00:29.407    6104-6104/utsunomiya.gclue.com.sensorsample D/SNESOR﹕ Sensor:Pedometer
10-23 09:00:29.407    6104-6104/utsunomiya.gclue.com.sensorsample D/SNESOR﹕ Sensor:PEDESTRIAN-ACTIVITY-MONITOR
```