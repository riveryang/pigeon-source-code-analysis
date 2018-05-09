# 项目介绍

Pigeon是一个分布式的RPC框架，由点评开发并开源，目前最新的版本为2.9.12-SNAPSHOT，很可惜的是Pigeon已经在2017年停止维护，但是作为学习还是非常有价值的。

Pigeon的服务开发有两种模式，Spring Schema和Annotation，两种模式对Spring都有强依赖，在使用过程中无论是使用Spring Schema或Annotation的方式，都需要依赖Spring。

![](/assets/简要架构.png)

Pigeon分为两部分，服务提供端和服务消费端，通过注册中心进行配置同步。

