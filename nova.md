1. Message Queue    
Nova使用的是一种“shared-nothing, messaging-based”架构，组件间通过消息队列进行通信。只要支持AMQP协议的任何Message Queue Sever都可以作为Nova组件间通信的管道，当前官方推荐用RabbitMQ。另外，为了提高用户体验，Nova使用“回调”(call-back)机制发送消息。    
  - [AMQP和RabbitMQ入门](http://www.infoq.com/cn/articles/AMQP-RabbitMQ)    
  - [关于RabbitMQ的概念，特性和使用](http://lynnkong.iteye.com/blog/1699684)    

2. nova-netwaork    
  - [OpenStack 用 iptables、链和规则处理联网](http://www.ibm.com/developerworks/cn/cloud/library/cl-openstack-network/)   
  - [Installation Prerequisites for Rackspace Private Cloud 2.0](http://www.rackspace.com/knowledge_center/article/installation-prerequisites-for-rackspace-private-cloud-20)    
  - [Rackspace Private Cloud Software - Creating an Instance in the Cloud](http://www.rackspace.com/knowledge_center/article/rackspace-private-cloud-software-creating-an-instance-in-the-cloud#compute-node-ssh)
    
