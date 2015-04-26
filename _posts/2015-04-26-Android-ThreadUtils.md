---
layout: post
title: Android ThreadUtils
category: technology
tags:
- Thread
- Android
- Utils
keywords: Android
description: 
---

### ThreadUtils


项目中有一些网络访问链接需要请求 每次开一个线程 还要写个handler比较麻烦 就想方便一些
于是就看看能不能写个工具类 将线程和一些可复用性强的东西写下来


{% highlight java%}

创建一个类 继承Runnadle


基本 参数
    /** handler处理 */
    private Handler handler;
    /** 网络请求地址 */
    private String url;
    /** index */
    private int index;



   /**
     * 构造方法
     *
     * @param handler
     *            消息对象
     * @param url
     *            请求的url地址
	 * @param index
     *            唯一标示
     */
    public HttpPostThread(Handler handler, String url,int index) {
        this.handler = handler;
        this.url = url;
        this.index = index;
    }


实现run方法
    @Override
    public void run() {
        // 获取我们回调主ui的message
        Message msg = handler.obtainMessage();
        try {

            Object result = HttpUtil.doPost(url);/访问网络

            msg.what = ConstantValues.GET_NET_SUCCEED;
            msg.obj = result;
            msg.arg1 = index;
        } catch (ClientProtocolException e) {
            msg.what = 404;
        } catch (IOException e) {
            msg.what = 100;
        }
        // 给主ui发送消息传递数据
        handler.sendMessage(msg);
    }



{% endhighlight %}

恩基本也就这样了 用的时候

{% highlight java%}
    String url = getUrl();
    ThreadUtils threadutils = new ThreadUtils(handler, url ,i);
    new Thread(ThreadUtils).start();
{% endhighlight%}

