---
layout: post
title: Android Popupwindow
category: technology
tags:
- popupwindow
- Android
- 自定义UI
keywords: Android
description: 
---

### popupwindow

popupwindow主要是用于弹出一个悬浮在页面上的提示框 可自拟定属性较多

动画,大小,位置,背景等等


在项目中styles.xml添加

{% highlight XML%}

    <style name="popupAnimation" parent="android:Animation">
        <item name="android:windowEnterAnimation">@anim/more_open</item>
        <item name="android:windowExitAnimation">@anim/more_close</item>
    </style>

{% endhighlight %}

more_open 开始动画
{% highlight XML%}

<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android">
    <translate
        android:fromYDelta="100%p"
        android:toYDelta="0"
        android:duration="100"
        />
</set>

{% endhighlight %}

more_open 结束动画
{% highlight XML%}

<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android">
    <translate
        android:fromYDelta="0"
        android:toYDelta="100%p"
        android:duration="100"
        />
</set>

{% endhighlight %}



java 方法
{% highlight java%}

    PopupWindow mPopupWindow;

    public void showpop(View v) {
        // 屏幕的width
        int mScreenWidth;
        
        // 屏幕的height
        int mScreenHeight;

        // PopupWindow的width
        int mPopupWindowWidth;

        // PopupWindow的height
        int mPopupWindowHeight;

        View popupWindow = LayoutInflater.from(v.getContext()).inflate(R.layout.dialog_more, null);

        Button shield = (Button) popupWindow.findViewById(R.id.hall_bon_shield);
        Button report = (Button) popupWindow.findViewById(R.id.hall_bon_report);
        Button collection = (Button) popupWindow.findViewById(R.id.hall_bon_collection);
        Button tvCancel = (Button) popupWindow.findViewById(R.id.hall_bon_off);
        tvCancel.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                mPopupWindow.dismiss();

            }
        });



        mScreenWidth = ScreenUtils.getScreenWidth(v.getContext());
        mScreenHeight = ScreenUtils.getScreenHeight(v.getContext());
        mPopupWindow = new PopupWindow(popupWindow, mScreenWidth, LinearLayout.LayoutParams.WRAP_CONTENT);
        mPopupWindow.setAnimationStyle(R.style.popupAnimation);
        mPopupWindow.setOutsideTouchable(true);
        mPopupWindow.setFocusable(true);
        mPopupWindow.setBackgroundDrawable(new ColorDrawable(0));
        mPopupWindowWidth = mPopupWindow.getWidth();
        mPopupWindowHeight = mPopupWindow.getHeight();
        mPopupWindow.showAtLocation(v, Gravity.BOTTOM, 0, 0);

    }


{% endhighlight %}




