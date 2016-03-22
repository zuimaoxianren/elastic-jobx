##Elastic-Jobx
* Elastic-Jobx在Elastic-Job的基础上提供了更低的入门门槛,更简单的配置和丰富的数据处理支持。
* Elastic-Jobx对Elastic-Job无侵入式改动，可随Elastic-Job升级。
* 作业命名空间使用约定前缀，监控平台可自动从zookeeper中获取作业信息，无需手工添加作业。
* 作业命名空间前缀可在监控平台修改。
* 增加数据处理中心，少量修改即可整合到自己业务中。
  
##主要贡献者
* 醉猫仙人 
* 微信公众号
 <img src="http://images.cnblogs.com/cnblogs_com/tenghoo/236809/o_zm.jpg" width = "200" height = "200" alt="醉猫仙人微信公众号" align=center />

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
