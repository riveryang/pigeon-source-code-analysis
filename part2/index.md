# Service使用的几种形式

* ReferenceBean

  * 基于Spring Schema的形式进行配置，通过配置与服务相对应的配置使其定位到具体服务，这种方式只能对每个服务进行单独配置

* AnnotationBean

  * 使用注解的形式注册服务，默认情况下只扫描 'com.dianping' 下的服务，我们可以通过Spring Schema（&lt;pigeon:annotation /&gt;）配置包扫描，Service服务加载使用注解@Reference，该注解支持简单的属性配置

# 几种形式示例

##### ReferenceBean：

```xml
<bean id="helloWorldService" class="com.dianping.pigeon.remoting.invoker.config.spring.ReferenceBean" init-method="init">
    <!-- 服务全局唯一的标识url，默认是服务接口类名，必须设置 -->
    <property name="url" value="http://service.pigeon.dianping.com/demo/helloWorldService_1.0.0" />
    <!-- 接口名称，必须设置 -->
    <property name="interfaceName" value="org.dianping.pigeon.service.HelloWorldService" />
    <!-- 超时时间，毫秒，默认5000，建议自己设置 -->
    <property name="timeout" value="2000" />
    <!-- 序列化，hessian/fst/protostuff，默认hessian，可不设置-->
    <property name="serialize" value="hessian" />
    <!-- 调用方式，sync/future/callback/oneway，默认sync，可不设置 -->
    <property name="callType" value="sync" />
    <!-- 失败策略，快速失败failfast/失败转移failover/失败忽略failsafe/并发取最快返回forking，默认failfast，可不设置 -->
    <property name="cluster" value="failfast" />
    <!-- 是否超时重试，默认false，可不设置 -->
    <property name="timeoutRetry" value="false" />
    <!-- 重试次数，默认1，可不设置 -->
    <property name="retries" value="1" />
</bean>
```

##### AnnotationBean：

```java
public class TestService {

    @Reference(url = "http://service.pigeon.dianping.com/demo/helloWorldService_1.0.0")
    private HelloWorldService helloWorldService;

}
```

# 核心类

* ReferenceBean
* ProviderBootStrap \( 公用、初始化JettyHttpServer \)
* ServiceFactory
* ServiceProxy
  * AbstractServiceProxy
    * DefaultServiceProxy
* InvokerBootStrap
  * ServiceInvocationRepository
  * InvokerProcessHandlerFactory
  * SerializerFactory
  * LoadBalanceManager
  * RegionPolicyManager
  * ResponseProcessorFactory
    * ResponseThreadPoolProcessor
  * Serializer
    * AbstractSerializer
* ClientManager
  * ProviderAvailableListener
  * ServiceProviderChangeListener
    * InnerServiceProviderChangeListener
  * RegistryConnectionListener
    * InnerRegistryConnectionListener
  * GroupChangeListener
    * InnerGroupChangeListener
* ClusterListenerManager
* RegistryManager
* RegistryEventListener
* ClusterListener
  * DefaultClusterListener
  * WeightFactorMaintainer
* ClientSelector
* ClientFactory
* Client
  * AbstractClient
    * NettyClient
    * HttpInvokeClient





