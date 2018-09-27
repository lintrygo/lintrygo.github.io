---
title: rabbitmq延迟队列简单实用
date: 2018-09-27 22:04:30
categories:
- 中间件
tags:
- rabbitmq
- 消息队列
---
  &nbsp; &nbsp; &nbsp; &nbsp;之前也没有具体研究过rabbitmq延迟队列的实现，前段时间在写短信提醒用户支付订单这个需求的时候，发现了一些问题。现有的商城系统，订单取消定时器是5分钟跑一次，这样就造成了会有5分钟的时间误差，前端给用户的提示以及后续的操作都会有影响。而且这种定时器一直跑着还是挺消耗系统资源的，增加数据库压力
  &nbsp; &nbsp; &nbsp; &nbsp;然后也是一直查阅资料看看有什么更好的解决方案，看到这本《Rabbitmq实战指南》写还的挺好的，通俗易懂，里面也简单介绍了延迟队列。
  

---
## 什么是延迟队列

&nbsp; &nbsp; &nbsp; &nbsp;延迟队列存储的对象肯定是对应的延迟消息，所谓”延迟消息”是指当消息被发送以后，并不想让消费者立即拿到消息，而是等待指定时间后，消费者才拿到这个消息进行消费。其实，AMQP协议，以及RabbitMQ本身没有直接支持延迟队列的功能，但是可以通过TTL和DLX模拟出延迟队列的功能。
## 过期时间TTL
TTL(Time To Live),即过期时间,RabbitMQ可以对消息和队列设置过期时间。<br>
消息在队列中的生存时间一旦超过了设置的TTL值，就会变为死信。

### 设置消息的TTL

```
byte[] messageBodyBytes = "Hello, world!".getBytes();
AMQP.BasicProperties properties = new AMQP.BasicProperties();
properties.setExpiration("60000");//设置消息的过期时间为60秒
channel.basicPublish("my-exchange", "routing-key", properties, messageBodyBytes);
```
### 设置队列的TTL

```
Map<String, Object> args = new HashMap<String, Object>();
args.put("x-expires", 1800000);
channel.queueDeclare("myqueue", false, false, false, args);

```

## DLX死信队列

```
Dead Letter Exchanges,可以称为死信交换器
channel.exchangeDeclare("exchange_delay_begin", "direct", true);
channel.exchangeDeclare("exchange_delay_done", "direct", true);

Map<String, Object> args = new HashMap<String, Object>();
args.put("x-dead-letter-exchange", "exchange_delay_done");
channel.queueDeclare("queue_delay_begin", true, false, false, args);
channel.queueDeclare("queue_delay_done", true, false, false, null);

channel.queueBind("queue_delay_begin", "exchange_delay_begin", "delay");
channel.queueBind("queue_delay_done", "exchange_delay_done", "delay");
```

这些内容我写的还是比较简单，后续会进一步的完善。建议阅读官方文档[http://www.rabbitmq.com/ttl.html#per-queue-message-ttl](https://note.youdao.com/)，或者上面提到的《RabbitMQ实战指南》,整个延迟队列的实现过程还是比较简单的，之后会把完整的代码贴出来。


















    
  
