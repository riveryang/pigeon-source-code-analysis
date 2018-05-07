# Provider的几种注册形式

* ServiceBean
  * 基于Spring Schema的形式进行配置，对于一些没有特殊要求的服务可以使用这种方式进行配置，通过services属性可以一次性注册多个Provider服务
* SingleServiceBean
  * 基于Spring Schema的形式进行配置，相对于ServiceBean，它支持更多的属性配置，它可以对每个方法进行单独的设置，使其支持对每个方法的线程池隔离，这种形式在配置上必须为每个Service都单独进行配置，而不像ServiceBean那样批量配置
* AnnotationBean
  * 使用注解的形式注册服务，默认情况下只扫描 'com.dianping' 下的服务，我们可以通过Spring Schema（&lt;pigeon:annotation /&gt;）配置包扫描，Provider的注册使用注解@Service，该注解支持简单的属性配置

# 核心类

* ServiceInitializeListener
  * ServiceBean
  * SingleServiceBean
  * AnnotationBean
* ServiceFactory
* PublishPolicy
  * AbstractPublishPolicy
    * DefaultPublishPolicy
* ServicePublisher
* InitializingService
* ServiceMethodFactory
* ProviderBootstrap
* Server
  * AbstractServer
    * NettyServer
    * JettyHttpServer
* NettyServerPipelineFactory
* NettyServerHandler
* RequestProcessor
  * RequestThreadPoolProcessor
* ThreadPool
  * DynamicThreadPool
* Runnable
  * HeartBeatListener
  * ServiceOnlineTask

# 时序图

### Server启动过程

![](/assets/Provider_Container_UP.png)

### 服务注册过程

![](/assets/ServicePublisher.png)

