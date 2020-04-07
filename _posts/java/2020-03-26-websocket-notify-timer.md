---
layout: post
title: WebSocket 定时推送消息
category: java
tags: [java,webSocket]
excerpt: WebSocket + quartz 实现消息持续推送
---

## 导入jar包
导入Tomcat 8 lib中WebSocket-api.jar

## 创建一个WebSocket 服务器
```java
package com.bxd.app.controller.biz.miniprogram;

import javax.websocket.OnClose;
import javax.websocket.OnError;
import javax.websocket.OnMessage;
import javax.websocket.OnOpen;
import javax.websocket.Session;
import javax.websocket.server.ServerEndpoint;
import java.io.IOException;
import java.util.concurrent.CopyOnWriteArraySet;

/**
 * @ServerEndpoint 注解是一个类层次的注解，它的功能主要是将目前的类定义成一个websocket服务器端,
 * 注解的值将被用于监听用户连接的终端访问URL地址,客户端可以通过这个URL来连接到WebSocket服务器端
 */
@ServerEndpoint("/websocket")
public class WebSocketServer {
	//静态变量，用来记录当前在线连接数。应该把它设计成线程安全的。
	private static int onlineCount = 0;

	//concurrent包的线程安全Set，用来存放每个客户端对应的MyWebSocket对象。若要实现服务端与单一客户端通信的话，可以使用Map来存放，其中Key可以为用户标识
	private static CopyOnWriteArraySet<WebSocketServer> webSocketSet = new CopyOnWriteArraySet<WebSocketServer>();

	//与某个客户端的连接会话，需要通过它来给客户端发送数据
	private Session session;

	/**
	 * 连接建立成功调用的方法
	 * @param session  可选的参数。session为与某个客户端的连接会话，需要通过它来给客户端发送数据
	 */
	@OnOpen
	public void onOpen(Session session){
		this.session = session;
		webSocketSet.add(this);     //加入set中
		addOnlineCount();           //在线数加1
		System.out.println("有新连接加入！当前在线人数为" + getOnlineCount());
	}

	/**
	 * 连接关闭调用的方法
	 */
	@OnClose
	public void onClose(){
		webSocketSet.remove(this);  //从set中删除
		subOnlineCount();           //在线数减1
		System.out.println("有一连接关闭！当前在线人数为" + getOnlineCount());
	}

	/**
	 * 收到客户端消息后调用的方法
	 * @param message 客户端发送过来的消息
	 * @param session 可选的参数
	 */
	@OnMessage
	public void onMessage(String message, Session session) {
		System.out.println("来自客户端的消息:" + message);
		//群发消息
		for(WebSocketServer item: webSocketSet){
			try {
				item.sendMessage(message);
			} catch (IOException e) {
				e.printStackTrace();
				continue;
			}
		}
	}

	/**
	 * 发生错误时调用
	 * @param session
	 * @param error
	 */
	@OnError
	public void onError(Session session, Throwable error){
		System.out.println("发生错误");
		error.printStackTrace();
	}

	/**
	 * 这个方法与上面几个方法不一样。没有用注解，是根据自己需要添加的方法。
	 * @param message
	 * @throws IOException
	 */
	public void sendMessage(String message) throws IOException{
		this.session.getBasicRemote().sendText(message);
		//this.session.getAsyncRemote().sendText(message);
	}

	public static synchronized int getOnlineCount() {
		return onlineCount;
	}

	public static synchronized void addOnlineCount() {
		WebSocketServer.onlineCount++;
	}

	public static synchronized void subOnlineCount() {
		WebSocketServer.onlineCount--;
	}

	public static CopyOnWriteArraySet<WebSocketServer> getWebSocketSet() {
		return webSocketSet;
	}

	public static void setWebSocketSet(
			CopyOnWriteArraySet<WebSocketServer> webSocketSet) {
		WebSocketServer.webSocketSet = webSocketSet;
	}

	public Session getSession() {
		return session;
	}

	public void setSession(Session session) {
		this.session = session;
	}

}


```

## 创建定时任务,推送消息
```java
package com.bxd.app.quartz;

import cn.hutool.core.util.RandomUtil;
import cn.hutool.core.util.StrUtil;
import cn.hutool.json.JSONUtil;
import com.bxd.app.constant.NotifyEnum;
import com.bxd.app.controller.biz.miniprogram.WebSocketServer;
import com.bxd.app.service.iface.biz.miniprogram.InformService;
import com.bxd.core.util.RedisUtil;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.scheduling.annotation.Scheduled;
import org.springframework.stereotype.Component;

import java.math.RoundingMode;
import java.util.Map;
import java.util.concurrent.CopyOnWriteArraySet;

@Component
public class GenNoticeJob {

	private static final Logger LOGGER =LoggerFactory.getLogger(GenNoticeJob.class);

	private static final String NAME_SEQUENCE = "李王张刘陈杨赵黄周吴徐孙胡朱高林何郭马罗梁宋郑谢韩唐冯于董萧程曹袁邓许傅沈曾彭吕苏卢蒋蔡贾丁魏薛叶阎余潘杜戴夏钟汪田任姜范方石姚谭廖邹熊金陆郝孔白崔康毛邱秦江史顾侯邵孟龙万段漕钱汤尹黎易常武乔贺赖龚文";

	private static final Integer[] arr = {5000,10000,15000,20000,50000,100000};


	@Autowired
	private InformService informService;


	/**
	 *  0 0/2 * * * ?
	 * 每两分钟随机生成消息记录
	 */
	@Scheduled(cron = "0/10 * * * * ?")
	public void genNotice() {

		Map<String, String> rs = RedisUtil.getRu().hgetall("notice");
		//获取WebSocketServer对象的映射。
		CopyOnWriteArraySet<WebSocketServer> webSocketSet = WebSocketServer.getWebSocketSet();
		if (webSocketSet.size() != 0) {
			for (WebSocketServer item : webSocketSet) {
				try {
					//向客户端推送消息
					item.getSession().getBasicRemote().sendText(JSONUtil.toJsonPrettyStr(rs));
				} catch (Exception e) {
					e.printStackTrace();
				}

			}

		}
	}
}

```

## 部署
部署到服务器,如果通过nginx 访问还需要添加配置
```
location /your_websocket_uri {
      proxy_http_version 1.1;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection "upgrade";
}
```