# Provider的几种注册形式

* ServiceBean
  * 基于Spring Schema的形式进行配置，对于一些没有特殊要求的服务可以使用这种方式进行配置，通过services属性可以一次性注册多个Provider服务
* SingleServiceBean
  * 基于Spring Schema的形式进行配置，相对于ServiceBean，它支持更多的属性配置，它可以对每个方法进行单独的设置，使其支持对每个方法的线程池隔离，这种形式在配置上必须为每个Service都单独进行配置，而不像ServiceBean那样批量配置
* AnnotationBean
  * 使用注解的形式注册服务，默认情况下只扫描 'com.dianping' 下的服务，我们可以通过Spring Schema（&lt;pigeon:annotation /&gt;）配置包扫描，Provider的注册使用注解@Service，该注解支持简单的属性配置

# 几种形式示例

##### ServiceBean：

```xml
<bean id="helloWorldService" class="org.dianping.pigeon.service.HelloWorldServiceImpl" />
<bean id="testService" class="org.dianping.pigeon.service.TestServiceImpl" />

<bean class="com.dianping.pigeon.remoting.provider.config.spring.ServiceBean" init-method="init">
    <property name="services">
        <map>
            <entry key="http://service.pigeon.dianping.com/demo/helloWorldService_1.0.0" value-ref="helloWorldService" />
            <entry key="http://service.pigeon.dianping.com/demo/testService_1.0.0" value-ref="testService" />
        </map>
    </property>
</bean>
```

##### SingleServiceBean：

```xml
<bean id="helloWorldService" class="org.dianping.pigeon.service.HelloWorldServiceImpl" />

<bean id="poolConfig" class="com.dianping.pigeon.remoting.provider.config.PoolConfig">
    <property name="poolName" value="Pigeon-Server-Pool-method" />
    <property name="corePoolSize" value="10" />
    <property name="maxPoolSize" value="30" />
    <property name="workQueueSize" value="100" />
</bean>

<bean id="helloWorldService.say" class="com.dianping.pigeon.remoting.provider.config.ProviderMethodConfig">
    <property name="name" value="say" />
    <property name="actives" value="10" />
    <property name="poolConfig" value-ref="poolConfig" />
</bean>

<bean class="com.dianping.pigeon.remoting.provider.config.spring.SingleServiceBean" init-method="init">
    <property name="serviceImpl" value-ref="helloWorldService" />
    <property name="url" value="http://service.pigeon.dianping.com/demo/helloWorldService_1.0.0" />
    <property name="methods">
        <list>
            <ref bean="helloWorldService.say" />            
        </list>
    </property>
</bean>
```

##### Annotation：

```java
import com.dianping.pigeon.remoting.provider.config.annotation.Service;

@Service(url = "http://service.pigeon.dianping.com/demo/helloWorldService_1.0.0")
public class HelloWorldServiceImpl implements HelloWorldService {

    @Override
    public String say() {
        return "WTF";
    }

}
```

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

![](/assets/服务端启动.png)

### 服务注册过程

![](/assets/服务注册%28发布%29.png)

