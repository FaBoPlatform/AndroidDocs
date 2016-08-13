# GPS

AndroidManifest.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="utsunomiya.gclue.com.gpssample" >

    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />

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

MainActivity.java
```java
package utsunomiya.gclue.com.gpssample;

import android.app.Activity;
import android.content.Context;
import android.content.Intent;
import android.location.Location;
import android.location.LocationListener;
import android.location.LocationManager;
import android.os.Bundle;
import android.provider.Settings;
import android.util.Log;
import android.view.Menu;
import android.view.MenuItem;
import android.widget.Toast;

public class MainActivity extends Activity implements LocationListener {

    /**
     * Location Manager
     */
    private LocationManager mLocationManager = null;

    /** Tag. */
    private final static String TAG = "GPS";

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // LocationManagerでGPSの値を取得するための設定
        mLocationManager = (LocationManager) getSystemService(Context.LOCATION_SERVICE);

        // GPSが有効かどうか確認する
        final boolean isGPS = mLocationManager.isProviderEnabled(LocationManager.GPS_PROVIDER);
        Log.i(TAG,"isGPS:" + isGPS);
        if(!isGPS){
            Intent mIntent = new Intent(Settings.ACTION_LOCATION_SOURCE_SETTINGS);
            startActivity(mIntent);
        }

        // 値が変化した際に呼び出されるリスナーの追加
        mLocationManager.requestLocationUpdates(LocationManager.GPS_PROVIDER, 1000, 50, this);

        Log.i(TAG,"GPS");
    }

    @Override
    protected void onPause() {
        super.onPause();

        if (mLocationManager != null) {
            mLocationManager.removeUpdates(this);
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
    public void onLocationChanged(Location location) {
        Log.i(TAG, "lat:" + location.getLatitude());
        Log.i(TAG, "lon:" + location.getLongitude());

        // トーストで表示.
        Toast.makeText(this, "lat:" + location.getLatitude() + "lon:" + location.getLongitude(), Toast.LENGTH_LONG).show();
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