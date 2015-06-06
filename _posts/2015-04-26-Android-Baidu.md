---
layout: post
title: Android 百度翻译api接口调用
category: technology
tags:
- 百度翻译
- api接口
- 项目设计
keywords: Android
description: 
---

Boss说 我们的软件必须国际化 要五国翻译,额 如果拼音也算一种的话 那是六国

调用接口识别目标想翻译成哪一国语言

{% highlight java %}
    private void getTranslate(String L) {
        new Thread(L) {
            public void run() {
                Message msg = new Message();
                Bundle data = new Bundle();
                String  longuage = "";
                if(tolonguage == "to_ch"){
                  longuage = HttpUtil.translate(add_sub.getText().toString(), HttpUtil.LONGUAGE_ZH);
                }else if(tolonguage == "to_jp"){
                    longuage =   HttpUtil.translate(add_sub.getText().toString(), HttpUtil.LONGUAGE_JP);
                }else if(tolonguage == "to_en"){
                    longuage =   HttpUtil.translate(add_sub.getText().toString(), HttpUtil.LONGUAGE_EN);
                }else if(tolonguage == "to_kor"){
                    longuage =   HttpUtil.translate(add_sub.getText().toString(), HttpUtil.LONGUAGE_KOR);
                }else if(tolonguage == "to_fra"){
                    longuage =   HttpUtil.translate(add_sub.getText().toString(), HttpUtil.LONGUAGE_FRA);
                }
                data.putString("value", longuage);
                msg.setData(data);
                // 发送一个消息 更新界面。
                handler.sendMessage(msg);
            };
        }.start();
    }
{% endhighlight%}


网络请求 返回json 并解析
{% highlight java %}
    /*
    中文	zh
    英语	en
    日语	jp
    法语	fra
    韩语	kor
    */
    public static String LONGUAGE_ZH = "zh";
    public static String LONGUAGE_JP = "jp";
    public static String LONGUAGE_EN = "en";
    public static String LONGUAGE_KOR = "kor";
    public static String LONGUAGE_FRA = "fra";
	
    public static String translate(String source, String language) {
        String api_url;
        try {
            api_url = new StringBuilder("http://openapi.baidu.com/public/2.0/bmt/translate?client_id=" + appkey + "&q=" + URLEncoder.encode(source, "utf-8") + "&from=auto&to=" + URLEncoder.encode(language, "utf-8")).toString();

            String json = Url2Json(api_url);
            LanguageMode translateMode = JSON.parseObject(json, LanguageMode.class);
            if (translateMode != null && translateMode.getTrans_result() != null && translateMode.getTrans_result().size() == 1) {
                return translateMode.getTrans_result().get(0).getDst();
            }
        } catch (Exception e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        return null;
    }
{% endhighlight%}





根据URL解析成字符串 并返回
{% highlight java %}
	
    public static String Url2Json(String http) {
        URL url;
        try {
            url = new URL(http);
            HttpURLConnection conn = (HttpURLConnection) url
                    .openConnection();

            conn.setRequestMethod("POST");
            // ANR
            conn.setConnectTimeout(4000);
            int code = conn.getResponseCode();
            if (code == 200) {

                InputStream stream = conn.getInputStream();
                String fromStream = StreamTools.readFromStream(stream);
                return fromStream;
            }
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (Exception e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        return null;
    }
	{% endhighlight%}
	
	
	
输入流InputStream  返回字符串
	{% highlight java %}
	
	   /**
     * @param is 输入流
     * @return String 返回的字符串
     * @throws IOException
     */
    public static String readFromStream(InputStream is) throws IOException {
        ByteArrayOutputStream baos = new ByteArrayOutputStream();
        byte[] buffer = new byte[1024];
        int len = 0;
        while ((len = is.read(buffer)) != -1) {
            baos.write(buffer, 0, len);
        }
        is.close();

        String result = URLDecoder.decode(baos.toString(), "UTF-8");
        baos.close();
        return result;
    }
	
	{% endhighlight%}
	
	
	
	解析json 所用到的 model
	{% highlight java %}
	
	package com.minitalk.model;
	 
	import java.io.Serializable;
	import java.util.List;
	 
	public class LanguageMode implements Serializable{

	  String from,to;

	  public String getFrom() {
		return from;
	  }
	  public void setFrom(String from) {
		this.from = from;
	  }
	  public String getTo() {
		return to;
	  }
	  public void setTo(String to) {
		this.to = to;
	  }

	  List<trans_result> trans_result;


	  public class trans_result implements Serializable{
		String dst,src;
	 
		public String getDst() {
		  return dst;
		}
	 
		public void setDst(String dst) {
		  this.dst = dst;
		}
	 
		public String getSrc() {
		  return src;
		}
	 
		public void setSrc(String src) {
		  this.src = src;
		}
	 
	  }
		public List<trans_result> getTrans_result() {
			return trans_result;
		}
		public void setTrans_result(List<trans_result> trans_result) {
			this.trans_result = trans_result;
		}
	  

	}
	{% endhighlight%}
	
	
	
恩 具体的操作就是这样 主要涉及json解析 和网络访问 都挺简单
拼音目前可以同开源框架pinyin4j 实现 可是boss 说要带声调的
目前虽然有些进展但还不完善就不放出来了