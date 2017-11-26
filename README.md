package com.example.test12;

import android.content.Context;
import android.content.res.TypedArray;
import android.graphics.Canvas;
import android.graphics.Color;
import android.graphics.LinearGradient;
import android.graphics.Matrix;
import android.graphics.Paint;
import android.graphics.Shader;
import android.util.AttributeSet;
import android.view.View;
import android.widget.TextView;

import org.w3c.dom.Attr;

public class textview extends android.support.v7.widget.AppCompatTextView {
   Paint mPaint1;
   Paint mPaint2;
int mViewWidth;
Paint mPaint;
LinearGradient mLinearGradient;
Matrix mGradientMatrix;
int mTranslate;

    public textview(Context context) {
        super(context);
        init();
    }

    public textview(Context context, AttributeSet attrs) {
        super(context, attrs);
        init();
    }

    public textview(Context context, AttributeSet attrs,
                  int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        init();
    }


@Override
protected void onDraw(Canvas canvas) {

    canvas.drawRect(0,0,getMeasuredWidth(),getMeasuredHeight(),mPaint1);
        canvas.drawRect(10,10,getMeasuredWidth()-10,getMeasuredHeight()-10,mPaint2);
        canvas.save();
        canvas.translate(10,0);
    super.onDraw(canvas);
    canvas.restore();                         //这个相当于一个合并图层作用，如果把操作全部放在下面就会导致文字被后来的矩形所覆盖
    if (mGradientMatrix !=null){
        mTranslate += mViewWidth / 5;
        if(mTranslate>2*mViewWidth){
            mTranslate=-mViewWidth;
        }
        mGradientMatrix.setTranslate(mTranslate,0);       //先改变矩阵，再改变渐变器
        mLinearGradient.setLocalMatrix(mGradientMatrix);
        postInvalidateDelayed(100);
    }
}




    private void init() {
        //初始化用来绘制背景边框的笔
        mPaint1=new Paint();
        mPaint2=new Paint();
        mPaint1.setColor(Color.RED);
        mPaint2.setColor(Color.GRAY);
        mPaint1.setStyle(Paint.Style.FILL);
        mPaint2.setStyle(Paint.Style.FILL);


    }

    @Override
    protected void onSizeChanged(int w, int h, int oldw, int oldh) {
        super.onSizeChanged(w, h, oldw, oldh);
        //闪动效果的对象初始化工作
        if(mViewWidth==0){
            mViewWidth=getMeasuredWidth();
            if(mViewWidth>0){
                //通过getPaint()方法获取绘制TextView的画笔
                mPaint=getPaint();//这个画笔是Textview的
                mLinearGradient= new LinearGradient(0,0,mViewWidth,0,
                        new int[]{Color.RED,Color.BLUE,Color.RED},
                        null, Shader.TileMode.CLAMP);
                //将该属性赋予给paint对象的shader渲染器
                mPaint.setShader(mLinearGradient);
                mGradientMatrix =new Matrix();//获取矩阵
            }
        }
    }




}
