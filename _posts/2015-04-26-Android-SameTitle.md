---
layout: post
title: Android同样的标题
category: technology
tags:
- 同样的标题
- 同一个标题栏
- Android标题栏
keywords: Android
description: 
---


由于项目需求 要求某几个Activity统一标题栏



新建 main_head.xml

{% highlight java %}

<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
	android:orientation="horizontal" 
	android:id="@+id/main_linearlayout_head"
	android:layout_width="fill_parent" 
	android:layout_height="wrap_content"
	android:gravity="center_vertical"
	>
          
        <Button
            android:id="@+id/btn_left"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="上一步"
            android:textSize="18sp" />

        <TextView
            android:id="@+id/tv_title"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:gravity="center"
            android:text="标题"
            android:textSize="18sp" />

        <Button
            android:id="@+id/btn_right"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="下一步"
            android:textSize="18sp" />
            
</LinearLayout>

{% endhighlight %}




接下来建立base_main.xml
 用来载入上面建立的 main_head.xml 如项目有多个不同 title bar 只需更换 include

{% highlight java%}

<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:background="#FFFFFF"
    android:orientation="vertical" >

    <include
        android:id="@+id/bottom"
        layout="@layout/main_header" />

    <LinearLayout
        android:id="@+id/content"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:layout_alignParentBottom="true"
        android:layout_below="@+id/bottom"
        android:orientation="horizontal" >



    </LinearLayout>

</RelativeLayout>

{% endhighlight %}

目前 视图需要建立的activity已经完成  我们建立一个BaseActivity 来管理

{% highlight java%}

package com.minitalk.adapter;

import android.app.Activity;
import android.content.Context;
import android.os.Bundle;
import android.util.Log;
import android.view.LayoutInflater;
import android.view.View;
import android.view.Window;
import android.widget.Button;
import android.widget.LinearLayout;
import android.widget.LinearLayout.LayoutParams;
import android.widget.TextView;

import com.minitalk.avtivity.R;

import framework.utils.AppManager;

/**
 * 继承于Activity用于以后方便管理
 *
 * @author coder
 */
public abstract class BaseActivity extends Activity {
    // 固定的菜单栏button
    private Button BackButton;
    private TextView Title;
    private Button NextButton;

    //所填从内容的布局
    private LinearLayout ly_content;

    // 内容区域的布局
    private View contentView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        requestWindowFeature(Window.FEATURE_NO_TITLE);
        setContentView(R.layout.base_main);
        ly_content = (LinearLayout) findViewById(R.id.content);

        BackButton = (Button) findViewById(R.id.btn_left);
        Title = (TextView) findViewById(R.id.tv_title);
        NextButton = (Button) findViewById(R.id.btn_right);

    }

    /**
     * 得到View的 内容
     *
     * @return
     */
    public View getLyContentView() {
        return contentView;
    }
    /**
     * 设置内容区域
     *
     * @param view View对象
     */
    public void setContentLayout(View view) {
        if (null != ly_content) {
            ly_content.addView(view);
        }
    }

    /**
     * 设置内容区域
     *
     * @param resId 资源文件ID
     */
    public View setContentLayout(int resId) {
        LayoutInflater inflater = (LayoutInflater) getSystemService(Context.LAYOUT_INFLATER_SERVICE);
        contentView = inflater.inflate(resId, null);

        LayoutParams layoutParams = new LayoutParams(LayoutParams.FILL_PARENT,
                LayoutParams.MATCH_PARENT);

        contentView.setLayoutParams(layoutParams);
        if (null != ly_content) {

            ly_content.addView(contentView);
            // 添加Activity到堆栈
            AppManager.getAppManager().addActivity(this);
            Log.v("AppManager",
                    "AppManager 添加actiivty！！" + this.getLocalClassName());
        }
        return contentView;
    }
    
    /**
     * 用来管理点击事件
    * */
    protected abstract void leftbnt();
    protected abstract void rightbnt();
    
    public void leftbnt(View view) {
        leftbnt();
    }

    public void rightbnt(View view) {
        rightbnt();
    }
    /**
     * 设置右上角的button消失
     */
    public void setnextgone() {
        NextButton.setVisibility(View.INVISIBLE);
    }
    /**
     * 设置标题
     */
    public void settitle(String title) {
        Title.setText(title);
    }




}

{% endhighlight %}

用于管理 activityStacks

{% highlight java %}

package com.example.zh_title;


import java.util.Stack;

import android.app.Activity;
import android.app.ActivityManager;
import android.content.Context;

public class AppManager {
    public static Activity context = null;

    private static Stack<Activity> activityStack;
    private static AppManager instance;

    private AppManager() {
    }

    /**
     * 单一实例
     */
    public static AppManager getAppManager() {
        if (instance == null) {
            instance = new AppManager();
        }
        return instance;
    }

    /**
     * 添加Activity到堆栈
     */
    public void addActivity(Activity activity) {
        if (activityStack == null) {
            activityStack = new Stack<Activity>();
        }
        activityStack.add(activity);
    }

    /**
     * 获取当前Activity（堆栈中最后一个压入的）
     */
    public Activity currentActivity() {
        Activity activity = activityStack.lastElement();
        return activity;
    }

    /**
     * 结束当前Activity（堆栈中最后一个压入的）
     */
    public void finishActivity() {
        Activity activity = activityStack.lastElement();
        finishActivity(activity);
    }

    /**
     * 结束指定的Activity
     */
    public void finishActivity(Activity activity) {
        if (activity != null) {
            activityStack.remove(activity);
            activity.finish();
            activity = null;
        }
    }

    /**
     * 结束指定类名的Activity
     */
    public void finishActivity(Class<?> cls) {
        for (Activity activity : activityStack) {
            if (activity.getClass().equals(cls)) {
                finishActivity(activity);
            }
        }
    }

    /**
     * 结束所有Activity
     */
    public void finishAllActivity() {
        for (int i = 0, size = activityStack.size(); i < size; i++) {
            if (null != activityStack.get(i)) {
                activityStack.get(i).finish();
            }
        }
        activityStack.clear();
    }

    /**
     * 退出应用程序
     */
    public void AppExit(Context context) {
        try {
            finishAllActivity();
            ActivityManager activityMgr = (ActivityManager) context
                    .getSystemService(Context.ACTIVITY_SERVICE);
            activityMgr.restartPackage(context.getPackageName());
            System.exit(0);
        } catch (Exception e) {
        }
    }
}

{% endhighlight %}




































