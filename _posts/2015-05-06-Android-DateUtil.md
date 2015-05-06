---
layout: post
title: Android dateUtils
category: technology
tags:
- date
- Android
- Utils
keywords: Android
description: 
---

时间差 :


 由于现在项目在做一个论坛类的APP 
	
 所发送的帖子需要显示时间距离现在过了多少
	
 所以顺手写了个工具类
	
 代码注释什么的都有我在这里就不废话了
	

{% highlight java %}

		/**
     * 将长时间格式字符串转换为时间 yyyy-MM-dd HH:mm:ss
     * @param strDate
     * @return
     */
    public static Date strToDateLong(String strDate) {
        SimpleDateFormat formatter = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        ParsePosition pos = new ParsePosition(0);
        Date strtodate = formatter.parse(strDate, pos);
        return strtodate;
    }

    /**
     * 时间显示
     *
     * @return
     */
    public static String getTimetext(String date) {

        String resultTime = "";


        long maxTime = System.currentTimeMillis();
        long minTime = strToDateLong(date).getTime();

        //秒数
        long cTime = (maxTime - minTime) / 1000;
        
        if (cTime < 60) {
            resultTime = "刚刚";
        } else if (cTime < 3600) {
            resultTime = (cTime / 60) + "分钟前";
        } else if (cTime < 86400) {
            resultTime = (cTime / 3600) + "小时前";
        } else if (cTime < 259200) {
            resultTime = (cTime / 86400) + "天前";
        } else {
            date = date.substring(5, 10);
            resultTime = "" + date;
        }

        return resultTime;

    }
{% endhighlight %}