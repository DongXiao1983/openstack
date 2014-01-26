1. Message Queue    
Nova使用的是一种“shared-nothing, messaging-based”架构，组件间通过消息队列进行通信。只要支持AMQP协议的任何Message Queue Sever都可以作为Nova组件间通信的管道，当前官方推荐用RabbitMQ。另外，为了提高用户体验，Nova使用“回调”(call-back)机制发送消息。    
[AMQP和RabbitMQ入门](http://www.infoq.com/cn/articles/AMQP-RabbitMQ)    
[关于RabbitMQ](http://lynnkong.iteye.com/blog/1699684)    
