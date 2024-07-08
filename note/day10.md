### WebSocket

WebSocket 是基于 TCP 的一种新的网络协议。它实现了浏览器与服务器全双工通信——浏览器和服务器只需要完成 一次握手，两者之间就可以创建持久性的连接， 并进行双向数据传输。 

HTTP协议和WebSocket协议对比：

* HTTP是短连接 
* WebSocket是长连接 ，长期维护需要成本
* HTTP通信是单向的，基于请求响应模式 
* WebSocket支持双向通信 
* HTTP和WebSocket底层都是TCP连接



* 来单提醒
* 客户催单



### 来电提醒

设计思路： 

* 通过WebSocket实现管理端页面和服务端保持长连接状态 
* 当客户支付后，调用WebSocket的相关API实现服务端向客户端推送消息 
* 客户端浏览器解析服务端推送的消息，判断是来单提醒还是客户催单，进行相应的消息提示和语音播报 
* 约定服务端发送给客户端浏览器的数据格式为JSON，字段包括：type，orderId，content 
* type 为消息类型，1为来单提醒 2为客户催单 orderId 为订单id content 为消息内容



```java
public void paySuccess(String outTradeNo) {
    // 当前登录用户id
    Long userId = BaseContext.getCurrentId();
    // 根据订单号查询当前用户的订单
    Orders ordersDB = orderMapper.getByNumberAndUserId(outTradeNo, userId);
    // 根据订单id更新订单的状态、支付方式、支付状态、结账时间
    Orders orders = Orders.builder()
                    .id(ordersDB.getId())
                    .status(Orders.TO_BE_CONFIRMED)
                    .payStatus(Orders.PAID)
                    .checkoutTime(LocalDateTime.now())
                    .build();
        orderMapper.update(orders);
        //////////////////////////////////////////////
        Map map = new HashMap();
        map.put("type", 1);//消息类型，1表示来单提醒
        map.put("orderId", orders.getId());
        map.put("content", "订单号：" + outTradeNo);
        //通过WebSocket实现来单提醒，向客户端浏览器推送消息
        webSocketServer.sendToAllClient(JSON.toJSONString(map));
        ///////////////////////////////////////////////////
}
```

### 客户催单

设计思路： 

* 通过WebSocket实现管理端页面和服务端保持长连接状态 
* 当用户点击催单按钮后，调用WebSocket的相关API实现服务端向客户端推送消息 
* 客户端浏览器解析服务端推送的消息，判断是来单提醒还是客户催单，进行相应的消息提示和语音播报 约定服务端发送给客户端浏览器的数据格式为JSON，字段包括：type，orderId，content 
* type 为消息类型，1为来单提醒 2为客户催单 orderId 为订单id content 为消息内容

```java
/**
* 用户催单
*
* @param id
*/
public void reminder(Long id) {
        // 查询订单是否存在
        Orders orders = orderMapper.getById(id);
        if (orders == null) {
        throw new OrderBusinessException(MessageConstant.ORDER_NOT_FOUND);
        }
        //基于WebSocket实现催单
        Map map = new HashMap();
        map.put("type", 2);//2代表用户催单
        map.put("orderId", id);
        map.put("content", "订单号：" + orders.getNumber());
        webSocketServer.sendToAllClient(JSON.toJSONString(map));
}

```



