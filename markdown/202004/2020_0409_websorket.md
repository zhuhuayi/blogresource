# 浅谈WebSocket

## 1.Websocket和Socket区别

## 2. Websocket 

客户端到客户端的通信，一定要在server端（中间站）sendMessage，另一端才能收到数据

## 3.通信例子

+ js java 互相发送信息
+ java-java 互相发送信息
+ java 推送消息到页面

js客户端

```js

<div style="height: 700px;overflow: auto;padding-top: 20px;" align='left' id="showData">
</div>

var debugToolSocket = null;
var data = [];
$("#connect").click(function(){
    if ('WebSocket' in window) {
        debugToolSocket = new WebSocket("ws://localhost:8080/websocket");
        // this.debugToolSocket = new SockJS("/sockjs/debugTool");

        debugToolSocket.onopen = function (event) {
            alert("start open");
        };
        debugToolSocket.onmessage = function (event) {
            pushData(event.data);
            if (!timer) {
                return;
            }
            this.timer = false;
        };
        debugToolSocket.onclose = function () {
            alert("start close");
        };
    } else {
        alert("this browse not support websocket");
    }
})

function pushData(eventData) {
    data.push(eventData);
    if (data.length > 50) {
        data.shift();
        $("#showData p").last().remove()
    }
    var pLabel = document.createElement("p");
    pLabel.innerHTML = eventData;
    $("#showData").prepend(pLabel);
}

```



java server端

```java
package com.saasbee.webapp.service;

import com.alibaba.fastjson.JSON;
import com.saasbee.webapp.domain.DebugToolSourceLog;
import com.saasbee.webapp.util.JsonUtil;
import org.apache.commons.lang.math.RandomUtils;
import us.zoom.common.RandomUtil;

import java.io.IOException;
import java.util.Random;
import java.util.concurrent.CopyOnWriteArraySet;

import javax.websocket.*;
import javax.websocket.server.ServerEndpoint;

/**
 * @ClassName WebsocketTest
 * @Author Michael Zhu
 * @Date 2020/4/9 10:51
 **/
@ServerEndpoint("/websocket")
public class WebsocketTest {

    //静态变量，用来记录当前在线连接数。应该把它设计成线程安全的。
    private static int onlineCount = 0;

    //concurrent包的线程安全Set，用来存放每个客户端对应的MyWebSocket对象。若要实现服务端与单一客户端通信的话，可以使用Map来存放，其中Key可以为用户标识
    public static CopyOnWriteArraySet<WebsocketTest> webSocketSet = new CopyOnWriteArraySet<WebsocketTest>();

    //与某个客户端的连接会话，需要通过它来给客户端发送数据
    private Session session;

    /**
     * 连接建立成功调用的方法
     * @param session  可选的参数。session为与某个客户端的连接会话，需要通过它来给客户端发送数据
     */
    @OnOpen
    public void onOpen(Session session) throws IOException {
        this.session = session;
        webSocketSet.add(this);     //加入set中
        addOnlineCount();           //在线数加1
        System.out.println("有新连接加入！当前在线人数为" + getOnlineCount());

        DebugToolSourceLog log = new DebugToolSourceLog();
        log.setMessage("AAAAAAAAAAAAAAAA" + RandomUtils.nextInt(100));
        this.session.getBasicRemote().sendText(JsonUtil.toJson(log));
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
        for(WebsocketTest item: webSocketSet){
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
        WebsocketTest.onlineCount++;
    }

    public static synchronized void subOnlineCount() {
        WebsocketTest.onlineCount--;
    }
}

```



参考地址：

[1]:	https://www.cnblogs.com/simuhunluo/p/8343334.html	" 汜慕魂落"