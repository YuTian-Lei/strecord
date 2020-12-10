## 业界主流消息中间件介绍

- ActiveMQ：基于JMS
- RabbitMQ：基于AMQP协议，erlang语言开发，稳定性好
- RocketMQ：基于JMS，阿里巴巴产品，目前交由Apache基金会
- Kafka：分布式消息系统，高吞吐量

## 消息中间件的优缺点

### 优点

- 解耦
- 异步
- 削峰

### 缺点

- 系统可用性降低
- 系统复杂性增加：如何保证保证消息可靠传输、如何保证消息不被重复消费、一致性问题

## RabbitMQ核心概念

### Exchange的内置类型

- fanout类型的Exchange是把收到的消息投到所有跟它绑定的Queue中
- direct类型的Exchange是精确匹配投递，把消息投到和它的RoutingKey相同的BindingKey对应的队列上
- topic类型的Exchange跟direct类型有点类似，但BindingKey可以用通配符（*匹配一个词，#表示匹配多个词），它把消息投递到所有跟消息RoutingKey匹配的Bindingkey对应的队列上，特别的BingingKey为#时，匹配所有的消息的RoutingKey，此时Exchange收到的所有消息都会投递到该队列中；
- header类型的Exchange是根据消息的header属性匹配，用的较少。

### Exchange与Queue关系

-  exchange 与 queue 是 多对多的关系，一个exchange上可以绑定多个queue；一个queue可以绑定到多个exchange上（多个exchange的类型可以不同，如一个是fanout, 一个是direct，一个是topic)。
- rabbitmq的queue跟其它mq服务器一样，可以有多个监听者，但是一个消息只能由一个监听者消费。

## RabbitMQ高级特性

### 消息100%投递

### 幂等性(避免重复消费)

### confirm

### return

### 消费端限流

### 消费端ACK和重回队列

### 死信队列

## RabbitMQ和Spring整合

##  构建RabbitMQ集群

