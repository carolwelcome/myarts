---
title: websocket完成服务器推送
date: 2019-05-05 22:38:22
categories:  #分类
    - attendance
tags:   #标签
    - ARTS
    - share 
    - Websocket
    - 服务器推送
description: 
    服务器推送
---

### 原理什么的后续会在此更新链接


### 具体实现

#### 前端js代码
```Javascript
<script type="text/javascript">
		var ws;
		if (typeof (WebSocket) == "undefined") {
			console.log("遗憾：您的浏览器不支持WebSocket");
		} else {
			console.log("恭喜：您的浏览器支持WebSocket");

			//实现化WebSocket对象
			//指定要连接的服务器地址与端口建立连接 
			//注意ws、wss使用不同的端口。我使用自签名的证书测试，
			//无法使用wss，浏览器打开WebSocket时报错
			//ws对应http、wss对应https。
			ws = new WebSocket("ws://localhost:8080/ws/asset");
			//连接打开事件  
			ws.onopen = function() {
				console.log("Socket 已打开");
				ws.send("消息发送测试(From Client)");  
			};
			//收到消息事件  
			ws.onmessage = function(msg) {
				console.log(msg.data);
			};
			//连接关闭事件  
			ws.onclose = function() {
				console.log("Socket已关闭");
			};
			//发生了错误事件  
			ws.onerror = function() {
				alert("Socket发生了错误");
			}

			//窗口关闭时，关闭连接
			window.unload=function() {
				ws.close();
			};
		}
</script>
```
#### websocket Server
三个类
1、WebSocketConfig,别看没多少，没这个你真不能跑起来
```Java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.socket.server.standard.ServerEndpointExporter;

@Configuration
public class WebSocketConfig {
    @Bean
    public ServerEndpointExporter serverEndpointExporter(){
        return new ServerEndpointExporter();
    }
}

```
2、WebSocketServer 正主来了
```Java
import java.io.IOException;
import java.util.concurrent.CopyOnWriteArraySet;
import java.util.concurrent.atomic.AtomicInteger;

import javax.websocket.OnClose;
import javax.websocket.OnError;
import javax.websocket.OnMessage;
import javax.websocket.OnOpen;
import javax.websocket.Session;
import javax.websocket.server.PathParam;
import javax.websocket.server.ServerEndpoint;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Component;


@ServerEndpoint(value = "/ws/asset")
@Component
public class WebSocketServer {
	
	private static Logger log = LoggerFactory.getLogger(WebSocketServer.class);  
    private static final AtomicInteger OnlineCount = new AtomicInteger(0);  
    // concurrent包的线程安全Set，用来存放每个客户端对应的Session对象。  
    private static CopyOnWriteArraySet<Session> SessionSet = new CopyOnWriteArraySet<Session>();  
  
  
    /** 
     * 连接建立成功调用的方法 
     */  
    @OnOpen  
    public void onOpen(Session session) {  
        SessionSet.add(session);   
        int cnt = OnlineCount.incrementAndGet(); // 在线数加1  
        log.info("有连接加入，当前连接数为：{}", cnt);  
        SendMessage(session, "连接成功");  
    }  
  
    /** 
     * 连接关闭调用的方法 
     */  
    @OnClose  
    public void onClose(Session session) {  
        SessionSet.remove(session);  
        int cnt = OnlineCount.decrementAndGet();  
        log.info("有连接关闭，当前连接数为：{}", cnt);  
    }  
  
    /** 
     * 收到客户端消息后调用的方法 
     *  
     * @param message 
     *            客户端发送过来的消息 
     */  
    @OnMessage  
    public void onMessage(String message, Session session) {  
        log.info("来自客户端的消息：{}",message);  
        SendMessage(session, "收到消息，消息内容："+message);  
  
    }  
  
    /** 
     * 出现错误 
     * @param session 
     * @param error 
     */  
    @OnError  
    public void onError(Session session, Throwable error) {  
        log.error("发生错误：{}，Session ID： {}",error.getMessage(),session.getId());  
        error.printStackTrace();  
    }  
  
    /** 
     * 发送消息，实践表明，每次浏览器刷新，session会发生变化。 
     * @param session 
     * @param message 
     */  
    public static void SendMessage(Session session, String message) {  
        try {  
            session.getBasicRemote().sendText(String.format("%s (From Server，Session ID=%s)",message,session.getId()));  
        } catch (IOException e) {  
            log.error("发送消息出错：{}", e.getMessage());  
            e.printStackTrace();  
        }  
    }  
  
    /** 
     * 群发消息 
     * @param message 
     * @throws IOException 
     */  
    public static void BroadCastInfo(String message) throws IOException {  
        for (Session session : SessionSet) {  
            if(session.isOpen()){  
                SendMessage(session, message);  
            }  
        }  
    }  
  
    /** 
     * 指定Session发送消息 
     * @param sessionId 
     * @param message 
     * @throws IOException 
     */  
    public static void SendMessage(String sessionId,String message) throws IOException {  
        Session session = null;  
        for (Session s : SessionSet) {  
            if(s.getId().equals(sessionId)){  
                session = s;  
                break;  
            }  
        }  
        if(session!=null){  
            SendMessage(session, message);  
        }  
        else{  
            log.warn("没有找到你指定ID的会话：{}",sessionId);  
        }  
    }  
      
}
```
3、WebSocketController，看名字说话吧
```Java
import java.io.IOException;  

import org.springframework.web.bind.annotation.RequestMapping;  
import org.springframework.web.bind.annotation.RequestMethod;  
import org.springframework.web.bind.annotation.RequestParam;  
import org.springframework.web.bind.annotation.RestController;  
  
/** 
 * WebSocket服务器端推送消息示例Controller 
 *  
 * 
 */  
@RestController  
@RequestMapping("/api/ws")  
public class WebSocketController {  
  
    @RequestMapping(value="/sendAll", method=RequestMethod.GET)  
    /** 
     * 群发消息内容 
     * @param message 
     * @return 
     */  
    String sendAllMessage(@RequestParam(required=true) String message){  
        try {  
            WebSocketServer.BroadCastInfo(message);  
        } catch (IOException e) {  
            e.printStackTrace();  
        }  
        return "ok";  
    }  
    @RequestMapping(value="/sendOne", method=RequestMethod.GET)  
    /** 
     * 指定会话ID发消息 
     * @param message 消息内容 
     * @param id 连接会话ID 
     * @return 
     */  
    String sendOneMessage(@RequestParam(required=true) String message,@RequestParam(required=true) String id){  
        try {  
            WebSocketServer.SendMessage(id,message);  
        } catch (IOException e) {  
            e.printStackTrace();  
        }  
        return "ok";  
    }  
}  
```


### 如何保证断线重连
导致服务断线的原因有很多，比如服务端重启、网络故障等等，但是作为一个实时给大屏推送数据的服务来说，通过让用户（或reload)这种方式来重新获取链接，实在是有点令人所不齿。熟悉websocket的人都知道，在连接关闭时会回调onclose方法。那么是不是可以这个onclose方法来做点文章呢？比如写个定时刷新的reload,亦或是不间断的发送重新链接请求到websocket server。这两种方式都实验过了，效果都不太好。前面一种也说过太low，后面这种想法根本就行不通。
可惜了，这么成熟的技术会给咱老李留这样的小坑，还真不信了。不就是面向浏览器编程吗？你说巧不巧，我找到了`reconnecting-websocket.js`,只需要
```Javascript
var ws = new WebSocket('ws://....');
//替换成下面
var ws = new ReconnectingWebSocket('ws://....');
```
问题很容易就解决了，下一期的tips来分享一下它的实现原理。