##Elastic-Jobx

  
##主要贡献者
* 醉猫仙人 
* 微信公众号
http://images.cnblogs.com/cnblogs_com/tenghoo/236809/o_zm.jpg
**讨论QQ群：**430066234（不限于Elastic-Job，包括分布式，定时任务相关以及其他互联网技术交流）

## Elastic-Job简介
[github](http://dangdangdotcom.github.io/elastic-job)
 

## Quick Start

* **引入maven依赖**

elastic-job已经发布到中央仓库，可以在pom.xml文件中直接引入maven坐标。

```xml
<!-- 引入elastic-job核心模块 -->
<dependency>
    <groupId>com.dangdang</groupId>
    <artifactId>elastic-job-core</artifactId>
    <version>${lasted.release.version}</version>
</dependency>

<!-- 使用springframework自定义命名空间时引入 -->
<dependency>
    <groupId>com.dangdang</groupId>
    <artifactId>elastic-job-spring</artifactId>
    <version>${lasted.release.version}</version>
</dependency>
```
* **作业开发**

```java
public class MyElasticJob extends AbstractSimpleElasticJob {
    
    @Override
    public void process(JobExecutionMultipleShardingContext context) {
        // do something by sharding items
    }
}
```

* **作业配置**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:reg="http://www.dangdang.com/schema/ddframe/reg"
    xmlns:job="http://www.dangdang.com/schema/ddframe/job"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
                        http://www.springframework.org/schema/beans/spring-beans.xsd
                        http://www.dangdang.com/schema/ddframe/reg
                        http://www.dangdang.com/schema/ddframe/reg/reg.xsd
                        http://www.dangdang.com/schema/ddframe/job
                        http://www.dangdang.com/schema/ddframe/job/job.xsd
                        ">
    <!--配置作业注册中心 -->
    <reg:zookeeper id="regCenter" serverLists=" yourhost:2181" namespace="dd-job" baseSleepTimeMilliseconds="1000" maxSleepTimeMilliseconds="3000" maxRetries="3" />

    <!-- 配置作业-->
    <job:bean id="myElasticJob" class="xxx.MyElasticJob" regCenter="regCenter" cron="0/10 * * * * ?"   shardingTotalCount="3" shardingItemParameters="0=A,1=B,2=C" />
</beans>
```
