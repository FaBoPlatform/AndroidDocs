# タッチイベント

## タッチイベント

タッチイベントは、Viewを継承したクラスで、onTouchEvent()を呼び出す事で、タッチされた座標の位置を取得する事ができるようになる。

MyView.java
```java
package com.gclue.viewsample;

import android.content.Context;
import android.content.res.Resources;
import android.graphics.Bitmap;
import android.graphics.BitmapFactory;
import android.graphics.Canvas;
import android.graphics.Color;
import android.graphics.Paint;
import android.view.MotionEvent;
import android.view.View;

/**
 * 描画用のクラス.
 */
class MyView extends View {

    /**
     * 画像を格納する変数.
     */
    private Bitmap myBitmap;

    /**
     * タグ.
     */
    private final static String TAG = "DRAW";

    /**
     * X.
     */
    private int x;

    /**
     * Y.
     */
    private int y;

    /**
     * コンストラクタ.
     *
     * @param context コンテキスト
     */
    public MyView(Context context) {
        super(context);
        setFocusable(true);

        // Resourceインスタンスの生成
        Resources res = this.getContext().getResources();
        // 画像の読み込み(res/drawable/gclue_logo.gif)
        myBitmap = BitmapFactory.decodeResource(res, R.drawable.gclue_logo);
    }

    /**
     * 描画処理を行う.
     */
    @Override
    protected void onDraw(Canvas canvas ) {
        super.onDraw(canvas);

        // 背景色を設定.
        canvas.drawColor(Color.BLUE);

        // 描画するための線の色を設定.
        Paint mPaint = new Paint();
        mPaint.setStyle( Paint.Style.FILL );
        mPaint.setARGB( 255, 255, 0, 0 );

        // Bitmapイメージの描画.
        canvas.drawBitmap(myBitmap, x, y, mPaint);
    }

    /**
     * タッチイベント.
     */
    @Override
    public boolean onTouchEvent( MotionEvent event ) {
        // タッチした時に実行
        if ( event.getAction() == MotionEvent.ACTION_DOWN ) {
            // X,Y座標の取得
            x = (int) event.getX();
            y = (int) event.getY();
            // 再描画の指示
            invalidate();
        }
        return true;
    }
}
```

## タッチイベントの種類

| 変数名 | 処理内容 |
| -- | -- |
| MotionEvent.ACTION_DOWN | タッチされた際に取得できる |
| MotionEvent.ACTION_MOVE | タッチされたあとの移動中に取得できる |
| MotionEvent.ACTION_UP | タッチが話された際に取得できる


```java
    /**
     * タッチイベント.
     */
    @Override
    public boolean onTouchEvent( MotionEvent event ) {
        // タッチした時に実行
        if ( event.getAction() == MotionEvent.ACTION_DOWN ) {
            // X,Y座標の取得
            x = (int) event.getX();
            y = (int) event.getY();
            // 再描画の指示
            invalidate();
        } else if ( event.getAction() == MotionEvent.ACTION_MOVE ) {
            // X,Y座標の取得
            x = (int) event.getX();
            y = (int) event.getY();
            // 再描画の指示
            invalidate();
        }
        return true;
    }
```
