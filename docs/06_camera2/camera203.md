# Cameraをオープン

## CameraのOpen
StateCallbackの変数を定義し、CameraをOpenします。StateCallbackのonOpen()で取得できるdeviceを用いて、カメラのセッションを生成していきます。

MainActivity.java
```java
package utsunomiya.gclue.com.camerasample;

import android.app.Activity;
import android.content.Context;
import android.graphics.SurfaceTexture;
import android.os.Bundle;
import android.util.Log;
import android.util.Size;
import android.view.Surface;
import android.view.TextureView;
import android.hardware.camera2.*;
import android.hardware.camera2.params.StreamConfigurationMap;
import android.hardware.camera2.CameraCharacteristics;

public class MainActivity extends Activity implements TextureView.SurfaceTextureListener {

    /**
     * TextureView
     */
    private TextureView mTextureView;
    /**
     * SurfaceTexture
     */
    private SurfaceTexture mSurfaceTexture;
    /**
     * Surface
     */
    private Surface mSurface;
    /**
     * CameraSize
     */
    private Size mCameraSize;
    /**
     * Log用のTag
     */
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

    // CameraDeviceのStateCallback
    CameraDevice.StateCallback mDeviceCallback = new CameraDevice.StateCallback()
    {
        @Override
        public void onDisconnected(CameraDevice device){
            Log.i(TAG, "onDisconnected");

        }

        @Override
        public void onError(CameraDevice device, int error){
            Log.i(TAG, "onError");

        }

        @Override
        public void onOpened(CameraDevice device){
            Log.i(TAG, "onOpened");

        }
    };

    @Override
    public void onSurfaceTextureAvailable(SurfaceTexture surface, int width, int height) {

        Log.i(TAG, "onSurfaceTextureAvailable");

        mSurfaceTexture = surface;
        mSurface = new Surface(mSurfaceTexture);

        try {
            CameraManager manager = (CameraManager) getSystemService(Context.CAMERA_SERVICE);
            for( String cameraId: manager.getCameraIdList()){

                Log.i(TAG, "cameraId:" + cameraId);

                // 取得したCameraIdからキャラスタリスチクスを取得
                CameraCharacteristics characteristics = manager.getCameraCharacteristics(cameraId);

                // Frontカメラの場合
                if (characteristics.get(CameraCharacteristics.LENS_FACING) == CameraCharacteristics.LENS_FACING_FRONT)
                {
                    Log.i(TAG, "FrontCamera");

                    // Mapを取得
                    StreamConfigurationMap map = characteristics.get(CameraCharacteristics.SCALER_STREAM_CONFIGURATION_MAP);

                    // サイズの一覧を取得
                    Size[] mOutputSize = map.getOutputSizes(SurfaceTexture.class);
                    for (int i = 0; i < mOutputSize.length; i++) {
                        Log.i(TAG, "size[" + i + "]" + mOutputSize[i].toString());
                    }

                    // カメラサイズ0番目をGlobalな関数に格納
                    mCameraSize = map.getOutputSizes(SurfaceTexture.class)[0];

                    // カメラをオープン
                    manager.openCamera(cameraId, mDeviceCallback, null);
                }
                // Backカメラの場合
                else if (characteristics.get(CameraCharacteristics.LENS_FACING) == CameraCharacteristics.LENS_FACING_BACK)
                {
                    Log.i(TAG, "BackCamera");
                }
            }
        }
        catch (Exception e) {
            Log.i(TAG, "Error:"+e);
        }
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

## CaptureSessionの生成

MainActivity.java
```java
package utsunomiya.gclue.com.camerasample;

import android.app.Activity;
import android.content.Context;
import android.graphics.SurfaceTexture;
import android.os.Bundle;
import android.util.Log;
import android.util.Size;
import android.view.Surface;
import android.view.TextureView;
import android.hardware.camera2.*;
import android.hardware.camera2.params.StreamConfigurationMap;
import android.hardware.camera2.CameraCharacteristics;

import java.util.Arrays;

public class MainActivity extends Activity implements TextureView.SurfaceTextureListener {

    /**
     * TextureView
     */
    private TextureView mTextureView;
    /**
     * SurfaceTexture
     */
    private SurfaceTexture mSurfaceTexture;
    /**
     * Surface
     */
    private Surface mSurface;
    /**
     * CameraSize
     */
    private Size mCameraSize;
    /**
     * CameraDevice
     */
    private CameraDevice mCameraDevice;
    /**
     * CaptureRequestBuilder
     */
    private CaptureRequest.Builder mCaptureRequestBuilder;

    /**
     * Log用のTag
     */
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

    // CameraDeviceのStateCallback
    CameraDevice.StateCallback mDeviceCallback = new CameraDevice.StateCallback()
    {
        @Override
        public void onDisconnected(CameraDevice device){
            Log.i(TAG, "onDisconnected");

        }

        @Override
        public void onError(CameraDevice device, int error){
            Log.i(TAG, "onError");

        }

        @Override
        public void onOpened(CameraDevice device){
            Log.i(TAG, "onOpened");

            // CaptureRequestの生成
            try {
                mCaptureRequestBuilder = mCameraDevice.createCaptureRequest(CameraDevice.TEMPLATE_PREVIEW);
            } catch (CameraAccessException e) {
                e.printStackTrace();
            }

            // Requestに取得したSurfaceを設定
            mCaptureRequestBuilder.addTarget(mSurface);

            // SurfaceTextureにサイズを設定
            mSurfaceTexture.setDefaultBufferSize(mCameraSize.getWidth(), mCameraSize.getHeight());

            // CaptureSessionの生成
            try {
                mCameraDevice.createCaptureSession(Arrays.asList(mSurface), mCaptureSessionCallback, null);
            } catch (CameraAccessException e) {
                e.printStackTrace();
            }
        }
    };

    // CameraCaptureSessionのStateCallback
    CameraCaptureSession.StateCallback mCaptureSessionCallback = new CameraCaptureSession.StateCallback()
    {
        @Override
        public void onConfigured (CameraCaptureSession session){
            Log.i(TAG, "onConfigured");
        }

        @Override
        public void onConfigureFailed (CameraCaptureSession session){
            Log.i(TAG, "onConfigureFailed");
        }
    };

    @Override
    public void onSurfaceTextureAvailable(SurfaceTexture surface, int width, int height) {

        Log.i(TAG, "onSurfaceTextureAvailable");

        mSurfaceTexture = surface;
        mSurface = new Surface(mSurfaceTexture);

        try {
            CameraManager manager = (CameraManager) getSystemService(Context.CAMERA_SERVICE);
            for( String cameraId: manager.getCameraIdList()){

                Log.i(TAG, "cameraId:" + cameraId);

                // 取得したCameraIdからキャラスタリスチクスを取得
                CameraCharacteristics characteristics = manager.getCameraCharacteristics(cameraId);

                // Frontカメラの場合
                if (characteristics.get(CameraCharacteristics.LENS_FACING) == CameraCharacteristics.LENS_FACING_FRONT)
                {
                    Log.i(TAG, "FrontCamera");

                    // Mapを取得
                    StreamConfigurationMap map = characteristics.get(CameraCharacteristics.SCALER_STREAM_CONFIGURATION_MAP);

                    // サイズの一覧を取得
                    Size[] mOutputSize = map.getOutputSizes(SurfaceTexture.class);
                    for (int i = 0; i < mOutputSize.length; i++) {
                        Log.i(TAG, "size[" + i + "]" + mOutputSize[i].toString());
                    }

                    // カメラサイズ0番目をGlobalな関数に格納
                    mCameraSize = map.getOutputSizes(SurfaceTexture.class)[0];

                    // カメラをオープン
                    manager.openCamera(cameraId, mDeviceCallback, null);
                }
                // Backカメラの場合
                else if (characteristics.get(CameraCharacteristics.LENS_FACING) == CameraCharacteristics.LENS_FACING_BACK)
                {
                    Log.i(TAG, "BackCamera");
                }
            }
        }
        catch (Exception e) {
            Log.i(TAG, "Error:"+e);
        }
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

## RepestSessionの生成

MainActivity.java
```java
package utsunomiya.gclue.com.camerasample;

import android.app.Activity;
import android.content.Context;
import android.graphics.SurfaceTexture;
import android.os.Bundle;
import android.util.Log;
import android.util.Size;
import android.view.Surface;
import android.view.TextureView;
import android.hardware.camera2.*;
import android.hardware.camera2.params.StreamConfigurationMap;
import android.hardware.camera2.CameraCharacteristics;

import java.util.Arrays;

public class MainActivity extends Activity implements TextureView.SurfaceTextureListener {

    /**
     * TextureView
     */
    private TextureView mTextureView;
    /**
     * SurfaceTexture
     */
    private SurfaceTexture mSurfaceTexture;
    /**
     * Surface
     */
    private Surface mSurface;
    /**
     * CameraSize
     */
    private Size mCameraSize;
    /**
     * CameraDevice
     */
    private CameraDevice mCameraDevice;
    /**
     * CaptureRequestBuilder
     */
    private CaptureRequest.Builder mCaptureRequestBuilder;

    /**
     * Log用のTag
     */
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

    // CameraDeviceのStateCallback
    CameraDevice.StateCallback mDeviceCallback = new CameraDevice.StateCallback()
    {
        @Override
        public void onDisconnected(CameraDevice device){
            Log.i(TAG, "onDisconnected");

        }

        @Override
        public void onError(CameraDevice device, int error){
            Log.i(TAG, "onError");

        }

        @Override
        public void onOpened(CameraDevice device){
            Log.i(TAG, "onOpened");

            // Globalな値に(onPause時にcloseするため)
            mCameraDevice = device;

            // CaptureRequestの生成
            try {
                mCaptureRequestBuilder = mCameraDevice.createCaptureRequest(CameraDevice.TEMPLATE_PREVIEW);
            } catch (CameraAccessException e) {
                e.printStackTrace();
            }

            // Requestに取得したSurfaceを設定
            mCaptureRequestBuilder.addTarget(mSurface);

            // SurfaceTextureにサイズを設定
            mSurfaceTexture.setDefaultBufferSize(mCameraSize.getWidth(), mCameraSize.getHeight());

            // CaptureSessionの生成
            try {
                mCameraDevice.createCaptureSession(Arrays.asList(mSurface), mCaptureSessionCallback, null);
            } catch (CameraAccessException e) {
                e.printStackTrace();
            }
        }
    };

    // CameraCaptureSessionのStateCallback
    CameraCaptureSession.StateCallback mCaptureSessionCallback = new CameraCaptureSession.StateCallback()
    {
        @Override
        public void onConfigured (CameraCaptureSession session){
            Log.i(TAG, "onConfigured");

            try {
                session.setRepeatingRequest(mCaptureRequestBuilder.build(), mCaptureCallback , null);
            } catch (CameraAccessException e) {
                e.printStackTrace();
            }
        }

        @Override
        public void onConfigureFailed(CameraCaptureSession session) {
            Log.i(TAG, "onConfigureFailed");
        }
    };

    // CaptureSessionのCallback
    CameraCaptureSession.CaptureCallback mCaptureCallback = new CameraCaptureSession.CaptureCallback()
    {
        @Override
        public void onCaptureCompleted(CameraCaptureSession session, CaptureRequest request, TotalCaptureResult result) {

            Log.i(TAG, "onCaptureCompleted");

        }
    };

    @Override
    public void onSurfaceTextureAvailable(SurfaceTexture surface, int width, int height) {

        Log.i(TAG, "onSurfaceTextureAvailable");

        mSurfaceTexture = surface;
        mSurface = new Surface(mSurfaceTexture);

        try {
            CameraManager manager = (CameraManager) getSystemService(Context.CAMERA_SERVICE);
            for( String cameraId: manager.getCameraIdList()){

                Log.i(TAG, "cameraId:" + cameraId);

                // 取得したCameraIdからキャラスタリスチクスを取得
                CameraCharacteristics characteristics = manager.getCameraCharacteristics(cameraId);

                // Frontカメラの場合
                if (characteristics.get(CameraCharacteristics.LENS_FACING) == CameraCharacteristics.LENS_FACING_FRONT)
                {
                    Log.i(TAG, "FrontCamera");

                    // Mapを取得
                    StreamConfigurationMap map = characteristics.get(CameraCharacteristics.SCALER_STREAM_CONFIGURATION_MAP);

                    // サイズの一覧を取得
                    Size[] mOutputSize = map.getOutputSizes(SurfaceTexture.class);
                    for (int i = 0; i < mOutputSize.length; i++) {
                        Log.i(TAG, "size[" + i + "]" + mOutputSize[i].toString());
                    }

                    // カメラサイズ0番目をGlobalな関数に格納
                    mCameraSize = map.getOutputSizes(SurfaceTexture.class)[0];

                    // カメラをオープン
                    manager.openCamera(cameraId, mDeviceCallback, null);
                }
                // Backカメラの場合
                else if (characteristics.get(CameraCharacteristics.LENS_FACING) == CameraCharacteristics.LENS_FACING_BACK)
                {
                    Log.i(TAG, "BackCamera");
                }
            }
        }
        catch (Exception e) {
            Log.i(TAG, "Error:"+e);
        }
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

    @Override
    protected void onPause() {
        super.onPause();
        if (mCameraDevice == null) {
            return;
        }
        mCameraDevice.close();
    }
}
```