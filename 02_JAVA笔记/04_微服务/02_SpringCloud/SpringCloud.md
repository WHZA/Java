# 过去和未来

学完Spring Boot之后，我们有了这么一个疑问：当公司的服务器撑不住了怎么办？



以前有一个简单的解决方法：加服务器，实现负载均衡。

这样做十分简单粗暴，属于横向解决问题。

> 负载均衡就是将一个项目完整的复制一份，到另一台服务器上
>
> 努力实现两台服务器的负载均衡



但是假如一个公司有很多业务：签到，订单，支付，物流....

假如签到用的资源非常少，而其他资源用的非常多，那么虽然加服务器实现负载均衡可以解决这个问题，但是有非常多的资源会被浪费

我们想给签到少一点资源，给其他业务多一点资源



在这种情况下，出现了微服务架构体系，但是微服务架构存在一些问题：

**分布式架构会遇到的问题**：

1. 这么多服务分布在不同的服务器上，用户该如何去访问？

   > 解决：给用户一个相同的接口，一个共同的网管。然后用这个接口去分发到具体的服务器上

2. 这么多服务，如何进行通信？

3. 这么多服务，如何进行统一的管理？

   > 只要一个统一的管理平台即可

4. 服务器崩了怎么办？

   > 熔断机制



**解决方案**：Spring Cloud

> Spring Cloud是一套生态，而不是一个具体的解决方案
>
> 生态中的解决方案用来解决分布式的这些问题

1. Spring Cloud `NetFlix`：一站式解决方案

   1. 用户访问：`Api`网关，`zuul`组件

   2. 服务通信：`Feign`

      > `Feign`基于`HttpClient`
      >
      > `HttpClient`基于`Http`
      >
      > `Http`，同步并阻塞

   3. 统一管理：服务注册与发现：`Eureka`

   4. 服务器崩了：熔断机制：`Hystrix`

   > 虽然这个解决方案很好，但是在2018年年底，`NetFlix`宣布无限期停止维护
   >
   > 但是虽然不维护了，还是有很多东西还在用，比如熔断机制

2. `Apache Dubbo zookeeper`：半自动，需要整合别人

   1. `Api`网关，没有！所以要么找第三方，要么自己写
   2. 服务通信：`RPC`框架：`Dubbo`
   3. 服务注册与发现：`Zookeeper`(包含`Hadoop`,`Hive`)等
   4. 熔断机制：没有，借助了` Hystrix`

   > 这个解决方案不太完善，但是正在完善
   >
   > 比如`Dubbo3.0`，正在进行升级，将会成为`Apache`的顶级项目，是一种碾压性的有事

3. `Spring Cloud Alibaba`：一站式解决方案

   > 还没有孵化完成



---

比Spring Cloud更新一代的解决方案：服务网格

下一代微服务标准，`Server Mess`

代表解决方案：`istio`

---

## 常见面试题

1. 什么是微服务？
2. 微服务之间是如何进行通信的？
3. Spring Cloud和Dubbo的区别？
4. Spring Boot和Spring Cloud的理解？
5. 什么是服务熔断？什么是服务降级？
6. 微服务的优缺点？微服务有什么缺点？
7. 微服务的技术栈？
8. `Eureak`和`zookeeper`都可以进行注册与发现，他们的区别？

........



# 微服务概述

## 什么是微服务

微服务是近几年流行的一种服务器架构。

通常而言，微服务是一种架构模式或者说是一种架构风格。

他提倡将单一的应用程序划分为一组组小的服务，每个服务运行在单独的==进程==内，相互协调，提供最终价值。

服务之间==采用轻量级的通信机制进行互通==，每个服务围绕着具体的业务进行构建。

避免统一的，集中式的管理机制。

==可以选择不同的语言编写服务，也可以使用不同的数据存储。==

----

- 说白了，彻底解耦



## 微服务与微服务架构



**微服务**

强调的是一个服务的大小，它关注的是某一个点，解决某一个问题的服务应用

一个小小的组件



**微服务架构**

一种架构模式，一种思想



## 微服务优缺点

**优点**：

1. 单一职责
2. 每个服务足够内聚，足够小，代码容易理解，能够聚焦一个指定的业务或者需求
3. 开发简单，效率高，一个服务可能只做一件事
4. 可以被小团队开发，比如2~5个人
5. 松耦合，有功能意义的服务，无论是开发阶段还是部署阶段都是独立的
6. 可以使用不同语言进行开发
7. 容易和第三方进行集成，允许容易且灵活地方式集成自动部署，通过持续继承工具，比如`jenkins`，`Hudson`，`bamboo`
8. 容易被一个开发人员理解，修改和维护。
9. 允许融合最新技术
10. 只是纯业务逻辑代码，不用和`html`，`css`或者其他界面混合
11. ==每个服务都有自己的存储能力，可以有自己的数据库，也可以有统一数据库==



**缺点**：

1. 开发人员要处理分布式系统复杂性
2. 多服务运维难度随着服务的增加而增加
3. 系统部署依赖
4. 服务通信成本
5. 数据一致性
6. 系统集成测试
7. 性能监控



## 微服务技术栈

| 微服务条目                               | 落地技术                                                     |
| ---------------------------------------- | ------------------------------------------------------------ |
| 服务开发                                 | `SpringBoot`，`Spring`，`SpringMVC`                          |
| 服务配置与管理                           | `Neflix`的`Archaius`，`Alibaba`的`Diamond`等                 |
| 服务注册与发现                           | `Eureka`，`Consul`，`Zookeeper`等                            |
| 服务调用                                 | `Rest`，`RPC`，`gRPC`（Google）                              |
| 服务熔断器                               | `Hystrix`，`Envoy`等                                         |
| 负载均衡                                 | `Ribbon`，`Nginx`等                                          |
| 服务接口调用（客户端调用服务的简化工具） | `Feign`等                                                    |
| 消息队列                                 | `Kafka`，`RabbitMQ`，`ActiveMQ`等                            |
| 服务配置中心管理                         | `SpringCloud Config`，`Chef`等                               |
| 服务路由（API网关）                      | `Zuul`等                                                     |
| 服务监控                                 | `Zabbix`，`Nagios`，`Metrics`，`Specatator`等                |
| 全链路追踪                               | `Zipkin`，`Brave`，`Dapper`等                                |
| 服务部署                                 | `Docker`，`OpenStack`，`Kubernetes`等                        |
| 数据流操作开发包                         | `SpringCloud Steam`（封装`Redis，Rabbit，Kafka`等发送接收消息） |
| 事件消息总线                             | `SpringCloud Bus`                                            |

----

----

## 什么是Spring Cloud

Spring Cloud，基于Spring Boot提供了一套微服务解决方案，包括服务注册与发现，配置中心，全链路监控，服务网关，负载均衡，熔断器等组件，除了基于NetFlix的开源组件之外，还有一些选型中立的开源组建。

Spring Cloud利用SpringBoot的开发便利性，巧妙地简化了分布式系统基础设施的开发，SpringCloud为开发人员提供了快速构建分布式系统的一系列工具，包括配置管理，服务发现，断路器，路由，微代理，事件总线，全局锁，决策竞选，分布式会话等。他们都可以使用SpringBoot的开发风格做到一键启动和部署。



SpringCloud是分布式微服务架构下的一站式解决方案，是各个微服务架构落地技术的结合体，俗称微服务全家桶。



## SpringCloud和SpringBoot的关系

- SpringBoot专注于快速开发单个个体服务
- SpringCloud是关注全局的微服务协调整理治理框架，它将SpringBoot开发的一个个单体微服务整合并管理起来，为各个微服务提供：配置管理，服务发现，断路器，路由，微代理，事件总线，全局锁，决策竞选，分布式会话等集成服务
- SpringBoot可以离开SpringCloud独立使用，但是SpringCloud离不开Springboot
- SpringBoot专注于快速开发，方便的开发单体微服务，SpringCloud关注全局的服务之力框架



## Dubbo和SpringCloud的选择



Dubbo和SpringCloud的对比

|              | Dubbo         | SpringCloud                 |
| ------------ | ------------- | --------------------------- |
| 服务注册中心 | Zookeeper     | SpringCloud Netflix Eureka  |
| 服务调用方式 | RPC           | REST API                    |
| 服务监控     | Dubbo-monitor | Spring Boot Admin           |
| 断路器       | 不完善        | SpringCloud Netflix Hystrix |
| 服务网关     | 无            | SpringCloud Netflix Zuul    |
| 分布式配置   | 无            | SpringCloud Config          |
| 服务跟踪     | 无            | SpringCloud Sleuth          |
| 消息总线     | 无            | SpringCloud Bus             |
| 数据流       | 无            | SpringCloud Stream          |
| 批量任务     | 无            | SpringCloud Task            |

**最大区别：SpringCloud抛弃了Dubbo的RPC通信，采用的是基于HTTP的REST方式**

严格来说，这两种方式各有优劣，虽然从一定程度上来说，后者牺牲了服务调用的性能，但是也避免原生RPC带来的问题。

而且REST比RPC更加灵活。



**成品和组装的区别**

SpringCloud 比 Dubbo更加强大，涵盖面积更加广泛，而且作为Spring的拳头产品，他也可以与SpringFramework，SpringBoot，SpringData，SpringBatch等其他Spring项目相结合。

使用SpringCloud就像是使用品牌机，简单易上手。

使用Dubbo就像是使用组装机，更加灵活，对于新手更难。



**社区支持与更新力度**

Dubbo停止了5年左右的更新，虽然2017.7重启了，但是对于新技术的发展需要开发者自己拓展升级。比如当当网的DubboX。

但是这明显不现实，中小型企业没有能力去修改Dubbo的源码+一整套生态结构。



**解决的问题不同**

Dubbo的定位是一款RPC框架，而SpringCloud是微服务架构下的一站式解决方案

---

# SpringCloud的版本号

![1583840331082](SpringCloud.assets/1583840331082.png)

**上面的文档命名看起来很奇怪，所以我们需要了解：**

SpringCloud是由众多独立子项目组成的大型综合项目，每一个子项目都有着不同的发行节奏，都有自己的版本号。

所以为了避免混淆，没有通过版本号的方式，而是通过命名的方式来进行。



这些版本的命名方式使用了伦敦地铁站的名称，同时根据字母的顺序来进行版本时间排序。



比如最早的Release版本：Angel。第二个Release版本：Brixton。然后是：Camden、Dalston、Edgware。

现在已经到了H版本了



# 参考书

SpringCloudNetflix中文文档：https://www.springcloud.cc/spring-cloud-netflix.html

SpringCloud中文API：https://www.springcloud.cc/spring-cloud-dalston.html

SpringCloud中国社区：http://springcloud.cn/

SpringCloud中文网：https://www.springcloud.cc/



# 总体开始

SpringCloud的学习就打一个整体的项目，然后在这个项目下建立分项目即可

- 我们使用一个`Dept`部门模块做一个微服务通用案例`Consumer`消费者(浏览器)通过REST调用`Provider`提供者(服务器)提供的服务

- Maven的分包分模块架构复习：

  ```bash
  一个简单的Maven项目是这样的：
  
  -- app-parent：一个父项目(app-parent)，聚合了很多子项目(app-util，app-dao，app-web....)
    |-- pom.xml
    |
    |-- app-core
    ||--pom.xml
    |
    |-- app-web
    ||--pom.xml
    ....
  ```

  一个父工程带有多个子`Module`模块

# SpringCloud版本如何选择

| SpringBoot | SpringCloud   | 关系                         |
| ---------- | ------------- | ---------------------------- |
| 1.2.x      | Angel版本     | 兼容SpringBoot  1.2.x        |
| 1.3.x      | Brixton版本   | 兼容SpringBoot  1.3.x和1.4.x |
| 1.4.x      | Camden版本    | 兼容SpringBoot  1.4.x和1.5.x |
| 1.5.x      | Dalston版本   | 兼容SpringBoot 1.5.x         |
| 1.5.x      | Edgware版本   | 兼容SpringBoot  1.5.x        |
| 2.0x       | Finchley版本  | 兼容SpringBoot  2.0x         |
| 2.1x       | Greenwich版本 | 兼容Spring Boot 2.1.x        |

更详细的版本对应：



![1583844645326](SpringCloud.assets/1583844645326.png)

> 使用最后的这两个

----

# 搭建环境



## **总项目搭建**

1. 创建一个普通的maven项目：springcloud，然后干掉src目录

2. 总pom文件

   ```xml
    <!--打包方式现在改为pom方式打包-->
       <packaging>pom</packaging>
   
       <!--包版本的管理-->
       <properties>
           <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
           <maven.compiler.source>1.8</maven.compiler.source>
           <maven.compiler.target>1.8</maven.compiler.target>
           <junit.version>4.12</junit.version>
           <log4j.version>1.2.17</log4j.version>
           <lombok.version>1.16.18</lombok.version>
       </properties>
   
       <!--坐标管理：这个dependencyManagement是管理者，子项目可以使用-->
       <dependencyManagement>
           <dependencies>
               <dependency>
                   <groupId>org.springframework.cloud</groupId>
                   <artifactId>spring-cloud-alibaba-dependencies</artifactId>
                   <version>0.2.0.RELEASE</version>
                   <type>pom</type>
                   <scope>import</scope>
               </dependency>
               <!--springCloud的依赖-->
               <dependency>
                   <groupId>org.springframework.cloud</groupId>
                   <artifactId>spring-cloud-dependencies</artifactId>
                   <version>Greenwich.SR1</version>
                   <type>pom</type>
                   <scope>import</scope>
               </dependency>
               <!--SpringBoot-->
               <dependency>
                   <groupId>org.springframework.boot</groupId>
                   <artifactId>spring-boot-dependencies</artifactId>
                   <version>2.1.4.RELEASE</version>
                   <type>pom</type>
                   <scope>import</scope>
               </dependency>
               <!--数据库-->
               <dependency>
                   <groupId>mysql</groupId>
                   <artifactId>mysql-connector-java</artifactId>
                   <version>5.1.47</version>
               </dependency>
               <dependency>
                   <groupId>com.alibaba</groupId>
                   <artifactId>druid</artifactId>
                   <version>1.1.10</version>
               </dependency>
               <!--SpringBoot启动器-->
               <dependency>
                   <groupId>org.mybatis.spring.boot</groupId>
                   <artifactId>mybatis-spring-boot-starter</artifactId>
                   <version>1.3.2</version>
               </dependency>
               <!--日志测试~-->
               <dependency>
                   <groupId>ch.qos.logback</groupId>
                   <artifactId>logback-core</artifactId>
                   <version>1.2.3</version>
               </dependency>
               <dependency>
                   <groupId>junit</groupId>
                   <artifactId>junit</artifactId>
                   <version>${junit.version}</version>
               </dependency>
               <dependency>
                   <groupId>log4j</groupId>
                   <artifactId>log4j</artifactId>
                   <version>${log4j.version}</version>
               </dependency>
               <!--lombok-->
               <dependency>
                   <groupId>org.projectlombok</groupId>
                   <artifactId>lombok</artifactId>
                   <version>${lombok.version}</version>
               </dependency>
           </dependencies>
       </dependencyManagement>
   ```



## 子项目实体类



**对应pojo**



1. 新建一个maven项目：`springcloud-api`，对应实体类，进去之后干掉测试

2. `pom.xml`

   ```xml
       <dependencies>
           <dependency>
               <groupId>org.projectlombok</groupId>
               <artifactId>lombok</artifactId>
           </dependency>
       </dependencies>
   ```

   > 从这里开始引入的坐标都是父项目的坐标

3. 建立一个新的==数据库==：就叫db01

   ![1583846088392](SpringCloud.assets/1583846088392.png)

4. 在db01这个数据库上新建数据表

   ![1583846395111](SpringCloud.assets/1583846395111.png)

   ```mysql
   create table dept
   (
   	deptno bigint auto_increment,
   	dname varchar(60) null,
   	db_source varchar(60) null,
   	constraint dept_pk
   		primary key (deptno)
   )
   comment '部门表';
   ```

   > `deptno`：主键
   >
   > `dname`：部门名称
   >
   > `db_source`：数据库的名字，因为是分布式，所以有数据库的名字，我们要知道这个是从哪个数据库里出来的



5. 插入数据

   ```mysql
   # DATABASES()会读取当前数据库的名字
   insert into dept (dname, db_source) values ('开发部',DATABASE());
   insert into dept (dname, db_source) values ('人事部',DATABASE());
   insert into dept (dname, db_source) values ('财务部',DATABASE());
   insert into dept (dname, db_source) values ('市场部',DATABASE());
   insert into dept (dname, db_source) values ('运维部',DATABASE());
   ```

   ![1583914818129](SpringCloud.assets/1583914818129.png)

6. 新建实体类：`com.bean.pojo.Dept`

   ```java
   package com.bean.pojo;
   
   import lombok.AllArgsConstructor;
   import lombok.Data;
   import lombok.NoArgsConstructor;
   import lombok.experimental.Accessors;
   
   import java.io.Serializable;
   
   //实现序列化接口
   @Data
   @AllArgsConstructor
   @NoArgsConstructor
   @Accessors(chain = true)//这个注解代表着链式写法，说明Dept现在支持链式写法了
   public class Dept implements Serializable {
       private Long deptno;
       private String dname;
   
       /**
        * 这个数据存在哪个数据库的字段
        * 因为微服务，一个服务对应一个数据库，同一个信息可能存在不同的数据库
         */
       private String db_source;
   }
   ```



## 子项目生产者



**对应提供者**，8001是端口号

**对应dao，server，controller。作为服务提供**



1. 新建一个`maven`项目：`springcloud-provider-8001`，干掉test

2. `pom.xml`

   ```xml
       <dependencies>
           <!--我们需要拿到实体类，所以需要 api这个模块-->
           <dependency>
               <groupId>com.bean</groupId>
               <artifactId>springcloud-api</artifactId>
               <version>1.0-SNAPSHOT</version>
           </dependency>
   
           <!--junit-->
           <dependency>
               <groupId>junit</groupId>
               <artifactId>junit</artifactId>
           </dependency>
   
           <!--mysql-->
           <dependency>
               <groupId>mysql</groupId>
               <artifactId>mysql-connector-java</artifactId>
           </dependency>
           <!--数据源-->
           <dependency>
               <groupId>com.alibaba</groupId>
               <artifactId>druid</artifactId>
           </dependency>
           <!--logback，日志-->
           <dependency>
               <groupId>ch.qos.logback</groupId>
               <artifactId>logback-core</artifactId>
           </dependency>
           <!--mybatis-springboot启动器-->
           <dependency>
               <groupId>org.mybatis.spring.boot</groupId>
               <artifactId>mybatis-spring-boot-starter</artifactId>
           </dependency>
           <!--test-->
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-test</artifactId>
           </dependency>
           <!--web-->
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-web</artifactId>
           </dependency>
           <!--jetty，类似tomcat-->
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-jetty</artifactId>
           </dependency>
           <!--热部署-->
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-devtools</artifactId>
           </dependency>
       </dependencies>	
   ```

   > 生产者要链接数据库

3. `application.yaml`

   ```yaml
   # 端口号
   server:
     port: 8001
   
   # mybatis的配置，其中配置别名的时候虽然是上一个项目的包名，但是我们通过坐标拿到了这些
   mybatis:
     type-aliases-package: com.bean.pojo
     mapper-locations: classpath:mybatis/mapper/*.xml
     config-location: classpath:mybatis/mybatis-config.xml
   
   # spring配置
   spring:
     application:
       name: springcloud-provider-dept
     datasource:
       type: com.alibaba.druid.pool.DruidDataSource
       driver-class-name: org.gjt.mm.mysql.Driver
       url: jdbc:mysql://localhost:3306/db01?useUnicode=true&characterEncoding=utf-8&serverTimezone=Asia/Shanghai
       username: root
       password: root
   ```

4. `mybatis-config.xml`

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE configuration
           PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
   
   <configuration>
       <settings>
           <!--开启二级缓存-->
           <setting name="cacheEnabled" value="true"/>
       </settings>
   </configuration>
   ```

5. `DeptMapper.xml`

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   
   <mapper namespace="com.bean.dao.DeptDao">
   
       <insert id="addDept" parameterType="Dept">
           insert into dept (dname, db_source)
           values (#{dname},DATABASE())
       </insert>
   
       <select id="queryById" resultType="Dept" parameterType="Long">
           select * from dept where deptno = #{deptno};
       </select>
   
       <select id="queryAll" resultType="Dept">
           select * from dept;
       </select>
   
   </mapper>
   ```

6. `DeptDao`

   ```java
   package com.bean.dao;
   
   import com.bean.pojo.Dept;
   import org.apache.ibatis.annotations.Mapper;
   import org.springframework.stereotype.Repository;
   
   import java.util.List;
   
   @Mapper
   @Repository
   public interface DeptDao {
   
       public boolean addDept(Dept dept);
   
       public Dept queryById(Long id);
   
       public List<Dept> queryAll();
   }
   ```

7. `DeptService`

   ```java
   package com.bean.service;
   
   import com.bean.pojo.Dept;
   
   import java.util.List;
   
   public interface DeptService {
   
       public boolean addDept(Dept dept);
   
       public Dept queryById(Long id);
   
       public List<Dept> queryAll();
       
   }
   ```

8. `DeptServiceImpl`

   ```java
   package com.bean.service;
   
   import com.bean.dao.DeptDao;
   import com.bean.pojo.Dept;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.stereotype.Service;
   
   import java.util.List;
   
   @Service
   public class DeptServiceImpl implements DeptService {
   
       @Autowired
       private DeptDao deptDao;
   
       @Override
       public boolean addDept(Dept dept) {
           return deptDao.addDept(dept);
       }
   
       @Override
       public Dept queryById(Long id) {
           return deptDao.queryById(id);
       }
   
       @Override
       public List<Dept> queryAll() {
           return deptDao.queryAll();
       }
   }
   ```

9. `DeptController`

   ```java
   package com.bean.controller;
   
   import com.bean.pojo.Dept;
   import com.bean.service.DeptService;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.web.bind.annotation.GetMapping;
   import org.springframework.web.bind.annotation.PathVariable;
   import org.springframework.web.bind.annotation.PostMapping;
   import org.springframework.web.bind.annotation.RestController;
   
   import java.util.List;
   
   //提供restful服务
   @RestController
   public class DeptController {
   
       @Autowired
       private DeptService deptService;
   
       //添加
       @PostMapping("/dept/add")
       public boolean addDept(Dept dept){
           return deptService.addDept(dept);
       }
   
       @GetMapping("/dept/get/{id}")
       public Dept get(@PathVariable("id")Long id){
           return deptService.queryById(id);
       }
   
       @GetMapping("/dept/list")
       public List<Dept> queryAll() {
           return deptService.queryAll();
       }
   }
   ```

10. 新建一个主启动类来测试一下

    ```java
    package com.bean;
    
    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;
    
    @SpringBootApplication
    public class DeptProvider_8001 {
        public static void main(String[] args) {
            SpringApplication.run(DeptProvider_8001.class,args);
        }
    }
    ```

11. 测试完毕

    ![1583917402050](SpringCloud.assets/1583917402050.png)



## 子项目消费者

**对应消费者，一般消费者进去的都是默认端口，所以是80端口**



1. 新建一个项目：`springcloud-consumer-dept-80`

2. `pom.xml`

   ```xml
       <dependencies>
           <!--实体类-->
           <dependency>
               <groupId>com.bean</groupId>
               <artifactId>springcloud-api</artifactId>
               <version>1.0-SNAPSHOT</version>
           </dependency>
           <!--web-->
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-web</artifactId>
           </dependency>
           <!--热部署-->
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-devtools</artifactId>
           </dependency>
       </dependencies>
   ```

   > 消费者显然不用链接数据库

3. `application.yaml`

   ```yaml
   server:
     port: 80
   ```

   

4. `controller`

   ```java
   package com.bean.controller;
   
   import com.bean.pojo.Dept;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.stereotype.Controller;
   import org.springframework.web.bind.annotation.PathVariable;
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.ResponseBody;
   import org.springframework.web.client.RestTemplate;
   
   import java.util.List;
   
   @Controller
   public class DeptConsumerController {
   
       //消费者不应该存在service层，所以我们要想办法拿到service的东西
       //使用Restful方式，用http进行通信
       //RestTemplate，这个模版里面有很多方法供我们调用
       //(url,实体:Map，返回值类型：Class<T> responseType)
   
       @Autowired
       private RestTemplate restTemplate;
   
       //去这个地址拿到service
       private static final String REST_URL_PREFIX="http://localhost:8001";
   
       @RequestMapping("/consumer/dept/get/{id}")
       @ResponseBody
       public Dept get(@PathVariable("id")Long id){
           //去这个地址拿到对应的值，注意这里使用的是get方式
           //还有postForObject,patchForObject,delete,....
           return restTemplate.getForObject(REST_URL_PREFIX+"/dept/get"+id,Dept.class);
       }
   
       @RequestMapping("/consumer/dept/add")
       @ResponseBody
       public Dept add(Dept dept){
           return restTemplate.postForObject(REST_URL_PREFIX+"/dept/add",dept,Dept.class);
       }
   
       @RequestMapping("/consumer/dept/list")
       @ResponseBody
       public List<Dept> list(){
           return restTemplate.getForObject(REST_URL_PREFIX+"/dept/list",List.class);
       }
   
   }
   ```

   > 注意，这里的RestTemplate是在config中的

4. `config`

   ```java
   package com.bean.config;
   
   import org.springframework.context.annotation.Bean;
   import org.springframework.context.annotation.Configuration;
   import org.springframework.web.client.RestTemplate;
   
   @Configuration
   public class ConfigBean {
   
       @Bean
       public RestTemplate getRestTemplate(){
           return new RestTemplate();
       }
   
   }
   ```

5. 写一个主启动类，测试一下

   ```java
   package com.bean;
   
   
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   
   @SpringBootApplication
   public class DeptConsumer_80 {
       public static void main(String[] args) {
           SpringApplication.run(DeptConsumer_80.class,args);
       }
   }
   ```

   > 注意了，这里要开启消费者和提供者两个服务



---

# Eureka



## 什么是Eureka

- 服务注册与发现
- Netflix再设计Eureka时，遵循的AP原则
- Eureka是Netfix的一个子模块，也是核心模块之一
- Eureka是一个基于REST的服务



## 原理



**回顾Dubbo**

首先我们来回顾一下Dubbo

![1583978904766](SpringCloud.assets/1583978904766.png)



**Eureka的基本架构**

- SpringCloud封装了Netfix公司开发的Eureka模块来实现服务的注册与发现（对比zookeeper）
- Eureka采用了C/S架构的模式设计，EurekaServer作为服务注册功能的服务器，他是服务注册中心
- 系统中的其他微服务，使用Eureka的客户端连接到EurekaSever并维持心跳链接。这样系统的维护人员就可以通过EurekaServer来监控系统中各个微服务是否正常运行，并执行相关逻辑



**Eureka的两个组件**

- Eureka Server：服务的注册。节点启动之后会在Eureka中进行注册。
- Eureka Client：一个JAVA客户端，用于简化EurekaSever的交互。有一个内置的，具有轮询负载算法的负载均衡器。应用启动后，回想EurekaServer发送心跳(默认周期30秒)。假如EurekaServer在多个周期内没有接受到某个节点的心跳，EurekaServer会从服务注册表中把这个服务节点移除（默认周期90秒）



**三大角色**

- Eureka Server：提供服务的注册与发现，代替Zookeeper的功能
- Service Provider：将自身服务注册到Eureka中，从而使消费者能够找到。环境搭建中的生产者。
- Service Consumer：服务消费方从Eureka中获取注册服务列表，从而找到消费服务。环境搭建中的消费者



## Eureka环境搭建



1. 新建一个子项目：springcloud-eureka-7001

2. `pom.xml`

   ```xml
       <dependencies>
           <!--Eureka Server-->
           <dependency>
               <groupId>org.springframework.cloud</groupId>
               <artifactId>spring-cloud-starter-eureka-server</artifactId>
               <version>1.4.6.RELEASE</version>
           </dependency>
           <!--热部署-->
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-devtools</artifactId>
           </dependency>
       </dependencies>
   ```

   > 这里导入的Eureka的包是服务端的

3. `application.yaml`

   ```yaml
   server:
     port: 7001
   
   # Eureka配置
   
   eureka:
     instance:
       hostname: localhost # Eureka服务端的实例名字
     client:
       register-with-eureka: false # 表示是否向eureka注册中心注册自己，因为我们编写的本来就是Eureka注册中心服务器，所以这个不用注册
       fetch-registry: false # 假如为False，表示自己为注册中心
       service-url:  # 别人连他如何链接，监控页面，点击去发现默认是8761端口，但是我们要重写
         defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/ # http://localhost:7001/eureka/
   ```

4. 主启动类

   ```java
   package com.bean;
   
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.cloud.netflix.eureka.server.EnableEurekaServer;
   
   @SpringBootApplication
   @EnableEurekaServer //EurekaServer服务端主启动类
   public class EurekaServer_7001 {
       public static void main(String[] args) {
           SpringApplication.run(EurekaServer_7001.class,args);
       }
   }
   ```

5. 启动测试

   ![1583980587740](SpringCloud.assets/1583980587740.png)

   > 注意，我们访问页面的时候直接访问`localhost:7001`就可以了，注册的时候才访问`localhost:7001/eureka`

----

## Eureka提供者

## 使用Eureka启动注册

1. 给服务生产者`springcloud-provider-8001`加入`Eureka`依赖

   ```xml
           <!--Eureka-->
           <dependency>
               <groupId>org.springframework.cloud</groupId>
               <artifactId>spring-cloud-starter-eureka</artifactId>
               <version>1.4.6.RELEASE</version>
           </dependency>
   ```

   > 注意，我们刚才用的依赖是Eureka的服务端，这次要更改一下，作为生产者就不能导入服务端的包了
   >
   > 导入正常的Eureka包即可，也就是客户端的坐标

2. `application.yaml`

   ```yaml
   # 端口号
   server:
     port: 8001
   
   # mybatis的配置，其中配置别名的时候虽然是上一个项目的包名，但是我们通过坐标拿到了这些
   mybatis:
     type-aliases-package: com.bean.pojo
     mapper-locations: classpath:mybatis/mapper/*.xml
     config-location: classpath:mybatis/mybatis-config.xml
   
   # spring配置
   spring:
     application:
       name: springcloud-provider-dept
     datasource:
       type: com.alibaba.druid.pool.DruidDataSource
       driver-class-name: org.gjt.mm.mysql.Driver
       url: jdbc:mysql://localhost:3306/db01?useUnicode=true&characterEncoding=utf-8&serverTimezone=Asia/Shanghai
       username: root
       password: root
   
   # Eureka生产者端的配置
   eureka:
     client:
       service-url:
         defaultZone: http://localhost:7001/eureka/ # 注册中心地址
     instance:
       instance-id: springcloud-provider-dept8001 # 修改Eureka中默认描述信息
   ```

3. 主启动类

   ```java
   package com.bean;
   
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.cloud.netflix.eureka.EnableEurekaClient;
   
   @SpringBootApplication
   @EnableEurekaClient//在服务启动后自动注册到Eureka中
   public class DeptProvider_8001 {
       public static void main(String[] args) {
           SpringApplication.run(DeptProvider_8001.class,args);
       }
   }
   ```

4. 测试：启动7001和8001

   ![1583981542721](SpringCloud.assets/1583981542721.png)

   > 我们看到，服务已经注册进去了，其中
   >
   > - Application栏中的名字：spring.application.name: springcloud-provider-dept
   > - Status：eureka.instance.instance-id: springcloud-provider-dept8001

-----

## 添加监控信息

1. 还是服务生产者

2. `pom.xml`

   ```xml
         <!--监控信息-->
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-actuator</artifactId>
           </dependency>
   ```

3. 配置监控信息

   ```yaml
   # 端口号
   server:
     port: 8001
   
   # mybatis的配置，其中配置别名的时候虽然是上一个项目的包名，但是我们通过坐标拿到了这些
   mybatis:
     type-aliases-package: com.bean.pojo
     mapper-locations: classpath:mybatis/mapper/*.xml
     config-location: classpath:mybatis/mybatis-config.xml
   
   # spring配置
   spring:
     application:
       name: springcloud-provider-dept
     datasource:
       type: com.alibaba.druid.pool.DruidDataSource
       driver-class-name: org.gjt.mm.mysql.Driver
       url: jdbc:mysql://localhost:3306/db01?useUnicode=true&characterEncoding=utf-8&serverTimezone=Asia/Shanghai
       username: root
       password: root
   
   # Eureka生产者端的配置
   eureka:
     client:
       service-url:
         defaultZone: http://localhost:7001/eureka/ # 注册中心地址
     instance:
       instance-id: springcloud-provider-dept8001 # 修改Eureka中默认描述信息
   
   
   # 监控信息配置，可以监控我们整个服务
   info: 
     app.name: bean-project # 项目的名字，随便起
     company.name: blog.bean # 公司名字
   ```

4. 开启7001和8001

   ![1583982113566](SpringCloud.assets/1583982113566.png)

   ![1583982125562](SpringCloud.assets/1583982125562.png)

---

## Eureka自我保护机制

有的时候可能爆红了：

![1583982215142](SpringCloud.assets/1583982215142.png)

这个意思是好死不如赖活着，有的时候一个微服务崩了，Eureka不会立刻把他清理，会把信息保存着，直到过了周期或者能够重新链接之后，等到很长时间才会关闭



关闭自我保护机制：（不建议关闭）

```yaml
eureka: 
  server:
	enable-self-preservation: false
```

> 注意，我们不建议关闭，关闭之后会立刻清理注册表

---

## 获取微服务的信息

1. 我们在8001新建一个controller

```java
import org.springframework.cloud.client.discovery.DiscoveryClient;

@Autowired
private DiscoveryClient client;
//注册的进来的微服务，可以获取一些消息
@GetMapping("/dept/discovery")
public Object discovery(){
    //获得微服务列表的清单
    List<String> services = client.getServices();
    System.out.println("service==>"+services);

    //通过一个具体的微服务id获得这个微服务的信息
    List<ServiceInstance> instances = client.getInstances("SPRINGCLOUD-PROVIDER-DEPT");
    for (ServiceInstance instance : instances) {
        System.out.println((instance.getHost() + instance.getPort() + instance.getUri() + instance.getServiceId()));
    }
    return this.client;
}
```

> SPRINGCLOUD-PROVIDER-DEPT：
>
> ![1583983088075](SpringCloud.assets/1583983088075.png)

2. 在主启动类中

   ```java
   @SpringBootApplication
   @EnableEurekaClient
   @EnableDiscoveryClient //服务发现
   public class DeptProvider_8001 {
       public static void main(String[] args) {
           SpringApplication.run(DeptProvider_8001.class,args);
       }
   }
   ```

   > @EnableDiscoveryClient使用这个信息

3. 测试

   ![1583983269498](SpringCloud.assets/1583983269498.png)

> 这个功能在团队开发中非常有用

----

## Eureka服务集群

> 集群可以保证：当一个机子崩了之后，另外的机子可以正常使用

**开始之前**

集群的配置不太麻烦，但是如果都用一个localhost在逻辑上有点难理解，下面我们来欺骗一下自己的大脑：

`C:\Windows\System32\drivers\etc\hosts`

在这里面新添加：

```host
127.0.0.1       eureka7001.com
127.0.0.1       eureka7002.com
127.0.0.1       eureka7003.com
```



![1585126043505](SpringCloud.assets/1585126043505.png)

> 这个意思是：localhost我们可以使用`eureka7001.com`,`eureka7002.com`,`eureka7003.com`来代替了

> 这样做的目的是欺骗一下自己的大脑，模拟真实开发环境（在不同主机上）

----

Eureka服务集群的配置就是让一个主机链接另外几台主机，实现共同成为注册中心，防止一台损坏其他崩溃

![1585126478197](SpringCloud.assets/1585126478197.png)

- `7001	注册	7002,7003`

- `7002	注册	7001,7003`

- `7003	注册	7001,7002`

-----

下面我们来进行注册中心集群的配置：



在之前，我们进行Eureka环境搭建的时候就已经配置了一个端口为7001的注册中心，下面放一下配置：

```xml
    <dependencies>
        <!--Eureka Server-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-eureka-server</artifactId>
            <version>1.4.6.RELEASE</version>
        </dependency>
        <!--热部署-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
        </dependency>
    </dependencies>
```

> 注意这里导入的Eureka坐标是服务端的坐标，不是客户端的坐标

```yaml
server:
  port: 7001

# Eureka配置

eureka:
  instance:
    hostname: localhost # Eureka服务端的实例名字
  client:
    register-with-eureka: false # 表示是否向eureka注册中心注册自己，因为我们编写的本来就是Eureka服务器，所以这个不用注册
    fetch-registry: false # 假如为False，表示自己为注册中心
    service-url:  # 别人连他如何链接，监控页面，点击去发现默认是8761端口，但是我们要重写
      defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/ # http://localhost:7001/eureka/
```

----

下面我们按照这个配置，新增两个新的模块`springcloud-eureka-7002`,`springcloud-eureka-7003`

端口分别为==7002==和==7003==：



然后我们将7001的pom文件中的依赖复制到7002和7003



然后修改配置文件

1. 将配置文件复制到7002和7003（别忘记改变端口）

2. 以7001举例，7001、7002、7003的hostname和注册中心都要改

   ```yaml
   server:
     port: 7001
   
   # Eureka配置
   eureka:
     instance:
       hostname: eureka7001.com # Eureka服务端的实例名字，这里改变了
     client:
       register-with-eureka: false
       fetch-registry: false
       service-url:
         defaultZone: http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/
         # 这里注册另外的两个注册中心，这里一定要注意空格别多写
   ```

3. 修改8001服务提供者的配置文件

   ```yaml
   
   server:
     port: 8001
   
   mybatis:
     type-aliases-package: com.bean.pojo
     mapper-locations: classpath:mybatis/mapper/*.xml
     config-location: classpath:mybatis/mybatis-config.xml
   
   spring:
     application:
       name: springcloud-provider-dept
     datasource:
       type: com.alibaba.druid.pool.DruidDataSource
       driver-class-name: org.gjt.mm.mysql.Driver
       url: jdbc:mysql://localhost:3306/db01?useUnicode=true&characterEncoding=utf-8&serverTimezone=Asia/Shanghai
       username: root
       password: root
   
   # Eureka生产者端的配置
   eureka:
     client:
       service-url:
         defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/
         # 注册中心地址把7001,7002,7003都加上
     instance:
       instance-id: springcloud-provider-dept8001
   
   info:
     app.name: bean-project
     company.name: blog.bean
   ```

   

然后我们将主启动类在7002和7003写一份



启动7001,7002,7003,8001，查看服务集群和服务是否注册上

![1585127357651](SpringCloud.assets/1585127357651.png)

![1585127680335](SpringCloud.assets/1585127680335.png)

----

## 显示IP

![1585285673337](SpringCloud.assets/1585285673337.png)

```yaml
# Eureka配置
eureka:
  instance:
    prefer-ip-address: true # 显示ip
```

![1585285887418](SpringCloud.assets/1585285887418.png)

-----

----

# CAP原则及对比Zookeeper

**回顾CAP原则**

关系型数据库		-->		ACID

非关系型数据库	-->		CAP



**ACID**

- A：原子性
- C：一致性
- I：隔离性
- D：持久性



**CAP**

- C：强一致性
- A：可用性
- P：分区容错性



**CAP原则**

指在一个分布式系统中，CAP三个要素中只能同时存在两点，而不可能三点同时存在

- CA：单点集群，满足一致性，可用性的系统，通常可扩展性较差
- CP：满足一致性，分区容错性的系统，通常性能不是很高
- AP：满足可用性，分区容错性的系统，通常对一致性要求低一些

----

**Zookeeper中保证的是CP，Eureka保证的是AP**

**Zookeeper中的CP**

当向注册中心查询服务列表的时候，注册中心有两种情况：

- 直接挂掉
- 返回几分钟之前的注册信息

很明显，挂掉是我们更加不能容忍的，也就是说，服务注册功能可用性的要求要高于一致性。

但是Zookeeper会出现这样一种情况：当主节点挂掉之后，其余节点不能够立刻顶上，而是要先选举一个新的主节点，然后顶上。

在整个选取期间Zookeeper集群都是不可用的。



**Eureka的AP**

Eureka的AP原则首先保证了可用性。Eureka各个节点之间都是平等的，几个节点挂掉不会影响整体的工作，，只不过查询到的信息可能不是最新的。

另外Eureka还有一种自我保护机制，如果在15分钟内超过85%的节点都没有心跳，那么Eureka就认为客户端与注册中心之间出现了故障，此时会出现以下几种情况：

1. Eureka不再从注册列表中移除因为时间过长没有收到心跳而应该过期的服务
2. Eureka仍然可以接受新服务的注册和查询请求，但是不会被同步到其他节点上（即保证当前节点仍然可用）
3. 当网络稳定时，当前实例新的注册信息会被同步到其他节点中

---

---

# 负载均衡和Ribbin

## Ribbon是什么

- Spring Cloud Ribbon是基于Netflix Ribbon实现的一套==客户端负载均衡的工具==

- 简单的来说，Ribbon是Netflix发布的开源项目，主要功能是提供客户端的软件负载均衡算法，将NetFlix的中间层服务连接到一起。

- Ribbon的客户端组件提供一系列完整的配置项，如

  - 连接超时
  - 重试

  > 简单的来说，就是在配置文件中列出LoadBalancer（简称LB：负载均衡）后面的所有机器，Ribbon会自动的帮助你基于某种规则（如简单轮询，随机连接等）去连接这些机器。
  >
  > 我们也很容易使用Ribbon实现自定义的负载均衡算法

  > 简单轮询的意思是：用户第一次请求服务，那么就去第一个服务端，第二次去第二个，...，第n次去第n个
  >
  > 简单轮询这个算法是一种最低级的算法
  >
  > 随机连接这个算法类似HashMap的算法

## Ribbon能干嘛

- LB，即负载均衡（Load Balance），在微服务或则好分布式集群中经常用到的一种应用
- `负载均衡简单地来说就是将用户的请求平摊地分派到多个服务上，从而达到系统的HA（高可用）`
- 常见的负载均衡软件：Nginx，Lvs(中国人开发)，Apache+Tomcat等
- Dubbo和SpringCloud中都给我们提供了负载均衡，SpringCloud的负载均衡算法可以自定义
- 负载均衡简单分类
  - 集中式LB
    - 即在服务的消费方和提供方之间使用独立的LB设施，比如Nginx，由该设施负责把访问请求通过某种策略转发到服务的提供方
  - 进程式LB
    - 将LB逻辑集成到消费方，消费方从服务注册中心获知有哪些地址可用，然后自己再从这些地址中选出一个合适的服务器
    - ==Ribbon属于进程内LB==，它只是一个类库，集成与消费方进程，消费方通过它来获取到服务提供方的地址

----

## 环境搭建和消费者配置

从上面的介绍我们知道，Ribbon是消费方的进程，所以我们在`springcloud-consumer-dept-80`消费者中操作





1. 首先导入pom依赖

   ```xml
           <!-- Ribbon -->
           <dependency>
               <groupId>org.springframework.cloud</groupId>
               <artifactId>spring-cloud-starter-ribbon</artifactId>
               <version>1.4.6.RELEASE</version>
           </dependency>
   ```

   > 我们取搜索Ribbon Maven的时候要注意，出来了很多，注意别导入Netflix的，要导入SpringCloud的
   >
   > ![1585130327773](SpringCloud.assets/1585130327773.png)
   >
   > Eureka配置的也是客户端的依赖，也就是8001的那个依赖

2. 配置`application.yaml`

   ```yaml
   server:
     port: 80
   
   #Eureka配置
   eureka:
     client:
       register-with-eureka: false # 我们不向注册中心注册自己，因为是消费者不是提供者
       service-url:
         defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/
   ```

3. 在主启动类上加上Eureka的注解

   ```java
   @SpringBootApplication
   @EnableEurekaClient //注解
   public class DeptConsumer_80 {
       public static void main(String[] args) {
           SpringApplication.run(DeptConsumer_80.class,args);
       }
   }
   ```

4. 我们之前在`com.bean.config`中编写了ConfigBean，下面再次改造这个类，在方法上实现负载均衡

   ```java
   @Configuration
   public class ConfigBean {
   
       @Bean
       @LoadBalanced //Ribbon,负载负载均衡实现
       public RestTemplate getRestTemplate(){
           return new RestTemplate();
       }
   }
   ```

   > 因为我们的Controller都是使用的RestHttp的方式进行的通信，而使用Rest就要使用RestTemplate，那么我们直接在根源上实现负载均衡

5. 改造Controller

   ```java
   @Controller
   public class DeptConsumerController {
   
       @Autowired
       private RestTemplate restTemplate;
   
       /*
           private static final String REST_URL_PREFIX="http://localhost:8001";
           去这个地址拿到service，但是我们使用Ribbon的时候这个地址应该是一个变量而不应该每次都从这个地址拿取
           所以我们应该从配置文件中的三种选择中获取：
           http://eureka7001.com:7001/eureka/
           http://eureka7002.com:7002/eureka/
           http://eureka7003.com:7003/eureka/
           那么这个变量是什么呢？这个变量就是服务名字，也就是之前在8001服务提供者配置的 spring.application.name: springcloud-provider-dept
       */
       private static final String REST_URL_PREFIX="http://SPRINGCLOUD-PROVIDER-DEPT";
   
       @RequestMapping("/consumer/dept/list")
       @ResponseBody
       public List<Dept> list(){
           return restTemplate.getForObject(REST_URL_PREFIX+"/dept/list",List.class);
       }
   
   
   }
   ```

6. 启动7001，7002，7003，8001，80

7. 访问`http://localhost/consumer/dept/list`

   ![1585131923415](SpringCloud.assets/1585131923415.png)

   > 成功

----

结论：

Eureka和Ribbon结合之后，客户端可以不用关心地址了，因为都是从Application中拿取

---

---

## Ribbon实现负载均衡

> 在这之前，其实已经实现了负载均衡，也就是上一步在消费者上我们在代码上配置的，但是结果很不明显，为了让结果清晰明了，我们要扩增几个服务提供者和对应的数据库

我们考虑，要另外新增两个消费者意思一下，那么我们的思想是这样的：

![1585208087315](SpringCloud.assets/1585208087315.png)

---

**首先是数据库**

我们以db01作为参考，来新建另外的两个数据库db02和db03

下面放上db01中dept的表：

```mysql
create table dept
(
	deptno bigint auto_increment,
	dname varchar(60) null,
	db_source varchar(60) null,
	constraint dept_pk
		primary key (deptno)
)
comment '部门表';
```

```mysql
# DATABASES()会读取当前数据库的名字
insert into dept (dname, db_source) values ('开发部',DATABASE());
insert into dept (dname, db_source) values ('人事部',DATABASE());
insert into dept (dname, db_source) values ('财务部',DATABASE());
insert into dept (dname, db_source) values ('市场部',DATABASE());
insert into dept (dname, db_source) values ('运维部',DATABASE());
```

> 以这张表为基础，另外新建两个数据库，数据库中同样建立这张表，表的字段要求一模一样，这样才可以实现负载均衡

```mysql

CREATE DATABASE db02;

USE db02;

CREATE TABLE dept
(
	deptno BIGINT AUTO_INCREMENT,
	dname VARCHAR(60) NULL,
	db_source VARCHAR(60) NULL,
	CONSTRAINT dept_pk
		PRIMARY KEY (deptno)
)
COMMENT '部门表';

# DATABASES()会读取当前数据库的名字
INSERT INTO dept (dname, db_source) VALUES ('开发部',DATABASE());
INSERT INTO dept (dname, db_source) VALUES ('人事部',DATABASE());
INSERT INTO dept (dname, db_source) VALUES ('财务部',DATABASE());
INSERT INTO dept (dname, db_source) VALUES ('市场部',DATABASE());
INSERT INTO dept (dname, db_source) VALUES ('运维部',DATABASE());


CREATE DATABASE db03;

USE db03;

CREATE TABLE dept
(
	deptno BIGINT AUTO_INCREMENT,
	dname VARCHAR(60) NULL,
	db_source VARCHAR(60) NULL,
	CONSTRAINT dept_pk
		PRIMARY KEY (deptno)
)
COMMENT '部门表';

# DATABASES()会读取当前数据库的名字
INSERT INTO dept (dname, db_source) VALUES ('开发部',DATABASE());
INSERT INTO dept (dname, db_source) VALUES ('人事部',DATABASE());
INSERT INTO dept (dname, db_source) VALUES ('财务部',DATABASE());
INSERT INTO dept (dname, db_source) VALUES ('市场部',DATABASE());
INSERT INTO dept (dname, db_source) VALUES ('运维部',DATABASE());
```



----

**下面是新增服务提供者**

我们要新增服务提供者8002和8003，下面放一张8001的配置作为参考：

> - `pom.xml`
>
>   ```xml
>       <dependencies>
>           <!--我们需要拿到实体类，所以需要 api这个模块-->
>           <dependency>
>               <groupId>com.bean</groupId>
>               <artifactId>springcloud-api</artifactId>
>               <version>1.0-SNAPSHOT</version>
>           </dependency>
>   
>           <!--junit-->
>           <dependency>
>               <groupId>junit</groupId>
>               <artifactId>junit</artifactId>
>           </dependency>
>   
>           <!--mysql-->
>           <dependency>
>               <groupId>mysql</groupId>
>               <artifactId>mysql-connector-java</artifactId>
>           </dependency>
>           <!--数据源-->
>           <dependency>
>               <groupId>com.alibaba</groupId>
>               <artifactId>druid</artifactId>
>           </dependency>
>           <!--logback，日志-->
>           <dependency>
>               <groupId>ch.qos.logback</groupId>
>               <artifactId>logback-core</artifactId>
>           </dependency>
>           <!--mybatis-springboot启动器-->
>           <dependency>
>               <groupId>org.mybatis.spring.boot</groupId>
>               <artifactId>mybatis-spring-boot-starter</artifactId>
>           </dependency>
>           <!--test-->
>           <dependency>
>               <groupId>org.springframework.boot</groupId>
>               <artifactId>spring-boot-test</artifactId>
>           </dependency>
>           <!--web-->
>           <dependency>
>               <groupId>org.springframework.boot</groupId>
>               <artifactId>spring-boot-starter-web</artifactId>
>           </dependency>
>           <!--jetty，类似tomcat-->
>           <dependency>
>               <groupId>org.springframework.boot</groupId>
>               <artifactId>spring-boot-starter-jetty</artifactId>
>           </dependency>
>           <!--热部署-->
>           <dependency>
>               <groupId>org.springframework.boot</groupId>
>               <artifactId>spring-boot-devtools</artifactId>
>           </dependency>
>           <!--Eureka-->
>           <dependency>
>               <groupId>org.springframework.cloud</groupId>
>               <artifactId>spring-cloud-starter-eureka</artifactId>
>               <version>1.4.6.RELEASE</version>
>           </dependency>
>           <!--监控信息-->
>           <dependency>
>               <groupId>org.springframework.boot</groupId>
>               <artifactId>spring-boot-starter-actuator</artifactId>
>           </dependency>
>       </dependencies>
>   ```
>
> - `application.yaml`
>
>   ```yaml
>   
>   server:
>     port: 8001
>   
>   mybatis:
>     type-aliases-package: com.bean.pojo
>     mapper-locations: classpath:mybatis/mapper/*.xml
>     config-location: classpath:mybatis/mybatis-config.xml
>   
>   spring:
>     application:
>       name: springcloud-provider-dept
>     datasource:
>       type: com.alibaba.druid.pool.DruidDataSource
>       driver-class-name: org.gjt.mm.mysql.Driver
>       url: jdbc:mysql://localhost:3306/db01?useUnicode=true&characterEncoding=utf-8&serverTimezone=Asia/Shanghai
>       username: root
>       password: root
>   
>   # Eureka生产者端的配置
>   eureka:
>     client:
>       service-url:
>         defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/
>         # 注册中心地址把7001,7002,7003都加上
>     instance:
>       instance-id: springcloud-provider-dept8001
>   
>   info:
>     app.name: bean-project
>     company.name: blog.bean
>   ```
>
> - `resources/mybatis/mybatis-config.xml`
>
>   ```xml
>   <?xml version="1.0" encoding="UTF-8" ?>
>   <!DOCTYPE configuration
>           PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
>   
>   <configuration>
>       <settings>
>           <!--开启二级缓存-->
>           <setting name="cacheEnabled" value="true"/>
>       </settings>
>   </configuration>
>   ```
>
> - `resources/mybatis/mapper/DeptMapper.xml`
>
>   ```xml
>   <?xml version="1.0" encoding="UTF-8" ?>
>   <!DOCTYPE mapper
>           PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
>   
>   <mapper namespace="com.bean.dao.DeptDao">
>   
>       <insert id="addDept" parameterType="Dept">
>           insert into dept (dname, db_source)
>           values (#{dname},DATABASE())
>       </insert>
>   
>       <select id="queryById" resultType="Dept" parameterType="Long">
>           select * from dept where deptno = #{deptno};
>       </select>
>   
>       <select id="queryAll" resultType="Dept">
>           select * from dept;
>       </select>
>   
>   </mapper>
>   ```
>
> - `com.bean.controller.DeptController`
>
>   ```java
>   @RestController
>   public class DeptConrtoller {
>   
>       @Autowired
>       private DeptService deptService;
>   
>       //添加
>       @PostMapping("/dept/add")
>       public boolean addDept(Dept dept){
>           return deptService.addDept(dept);
>       }
>   
>       @GetMapping("/dept/get/{id}")
>       public Dept get(@PathVariable("id")Long id){
>           return deptService.queryById(id);
>       }
>   
>       @GetMapping("/dept/list")
>       public List<Dept> queryAll() {
>           return deptService.queryAll();
>       }
>   
>       @Autowired
>       private DiscoveryClient client;
>   
>       //注册的进来的微服务，可以获取一些消息
>       @GetMapping("/dept/discovery")
>       public Object discovery(){
>           //获得微服务列表的清单
>           List<String> services = client.getServices();
>           System.out.println("service==>"+services);
>   
>           //通过一个具体的微服务id获得这个微服务的信息
>           List<ServiceInstance> instances = client.getInstances("SPRINGCLOUD-PROVIDER-DEPT");
>           for (ServiceInstance instance : instances) {
>               System.out.println((instance.getHost() + instance.getPort() + instance.getUri() + instance.getServiceId()));
>           }
>           return this.client;
>       }
>   }
>   ```
>
> - `com.bean.dao.DeptDao`
>
>   ```java
>   @Mapper
>   @Repository
>   public interface DeptDao {
>   
>        boolean addDept(Dept dept);
>   
>        Dept queryById(Long id);
>   
>        List<Dept> queryAll();
>   
>   }
>   ```
>
> - `com.bean.service.DeptService`
>
>   ```java
>   public interface DeptService {
>       public boolean addDept(Dept dept);
>   
>       public Dept queryById(Long id);
>   
>       public List<Dept> queryAll();
>   }
>   
>   ```
>
> - `com.bean.service.DeptServiceImpl`
>
>   ```java
>   @Service
>   public class DeptServiceImpl implements DeptService {
>       @Autowired
>       private DeptDao deptDao;
>   
>       @Override
>       public boolean addDept(Dept dept) {
>           return deptDao.addDept(dept);
>       }
>   
>       @Override
>       public Dept queryById(Long id) {
>           return deptDao.queryById(id);
>       }
>   
>       @Override
>       public List<Dept> queryAll() {
>           return deptDao.queryAll();
>       }
>   }
>   ```



> 有几个要点注意一下：
>
> 1. `application.yaml`中的端口号和描述信息要修改，但是应用名称不应该修改，数据库也需要修改为对应的数据库，下面放一张配置作为对比
>
>    ```yaml
>    
>    server:
>      port: 8002 # 端口号需要修改一下
>    
>    mybatis:
>      type-aliases-package: com.bean.pojo
>      mapper-locations: classpath:mybatis/mapper/*.xml
>      config-location: classpath:mybatis/mybatis-config.xml
>    
>    spring:
>      application:
>        name: springcloud-provider-dept # 注意，服务的名称必须要一个，因为我们在Controller中获取的是这里的名称
>      datasource:
>        type: com.alibaba.druid.pool.DruidDataSource
>        driver-class-name: org.gjt.mm.mysql.Driver
>        url: jdbc:mysql://localhost:3306/db02?useUnicode=true&characterEncoding=utf-8&serverTimezone=Asia/Shanghai # 数据库也需要修改一下
>        username: root
>        password: root
>    
>    eureka:
>      client:
>        service-url:
>          defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/
>      instance:
>        instance-id: springcloud-provider-dept8002 #描述信息应该修改一下
>    
>    info:
>      app.name: bean-project
>      company.name: blog.bean
>    ```
>
> 2. 启动类名称修改，放一张配置
>
>    ```java
>    @SpringBootApplication
>    @EnableEurekaClient
>    @EnableDiscoveryClient
>    public class DeptProvider_8003 {
>    
>        public static void main(String[] args) {
>            SpringApplication.run(DeptProvider_8003.class,args);
>        }
>    }
>    
>    ```



考虑到电脑性能的问题，我们这样启动：

启动 7001注册中心

启动8001,8002,8003服务提供者

启动80消费者



最终结果：

![1585209522307](SpringCloud.assets/1585209522307.png)

> 这里放的是负载到8003的时刻，其实Ribbon是使用1,3,2这样的轮询来进行负载均衡的
>
> Ribbon最基础的算法就是轮询

----

---

## Ribbon的负载均衡算法自定义

### 前期准备

首先我们要了解`IRule`是一切的源头，这就是负载均衡算法的接口，它有一些实现类

- `RoundRobinRule`：默认的轮询算法
- `RandomRule`：随机
- `AvailabilityFilteringRule`：先过滤掉跳闸和访问故障的服务，然后从剩下活着的服务里进行轮询
- `RetryRule`：先进行轮询，假如服务获取失败会在指定的时间内进行重试

---

然后我们要知道，这些配置都是放到Ribbon下面来做的，也就是消费者Consumer-80上做的

---

所谓自定义算法，就是把别人的算法拿来改一下，就成了自己的算法。比如我们要根据`RandomRule`进行修改



我们从官网上查看自定义Ribbon算法的时候，会发现他说对应的实现类应该放到主启动类包的外面

虽然不知道为什么，但是我们还是要尊重官网，免得出现什么不必要的错误

----

### 算法的分析

1. 新建一个springcloud的包，将刚才的类都放到这个包下

   > ![1585211528328](SpringCloud.assets/1585211528328.png)

2. 新建一个包`com.bean.ribbonconfig`，我们将在这里进行Ribbon算法的配置

   > ![1585211586378](SpringCloud.assets/1585211586378.png)

3. 将`RandomRule`复制一份到`ribbonconfig`包下，然后把没用的去了，下面放一个精简版的：

   ```java
   package com.bean.ribbonconfig;
   
   import com.netflix.client.config.IClientConfig;
   import com.netflix.loadbalancer.AbstractLoadBalancerRule;
   import com.netflix.loadbalancer.ILoadBalancer;
   import com.netflix.loadbalancer.Server;
   
   import java.util.List;
   import java.util.concurrent.ThreadLocalRandom;
   
   public class RandomRule extends AbstractLoadBalancerRule {
   
   
       /*这个代码这么多，一看就是实现算法的地方*/
       public Server choose(ILoadBalancer lb, Object key) {
           if (lb == null) {   //假如没有负载均衡，就返回null
               return null;
           }
           Server server = null;
   
           while (server == null) {    //当没有服务的时候
               if (Thread.interrupted()) {
                   return null;
               }
               List<Server> upList = lb.getReachableServers(); //获取还活着的服务
               List<Server> allList = lb.getAllServers();  //获取所有的服务
   
               
               /*-------------------------------------------------*/
               /*我们知道，这下面就是具体的算法，上边的都是前期的准备工作*/
               
               int serverCount = allList.size();
               if (serverCount == 0) {
                   return null;
               }
   
               int index = chooseRandomInt(serverCount);   //这个是索引，用于指向当前用哪个服务
               server = upList.get(index); //让服务 = 从活着的服务里面获取新的服务
               
               
               /*---------------------------------------------*/
               /*上面的就是具体的算法，下面就是一些收尾*/
               
               if (server == null) {   //假如服务没有获取到，还为null
                   Thread.yield();     //进行处理，咋处理的也看不懂....
                   continue;
               }
   
               if (server.isAlive()) { //假如这个服务还活着
                   return (server);    //返回
               }
   
               server = null;  //服务置空
               Thread.yield(); //处理
           }
   
           return server;
   
       }
   
   
       /*下面这三个方法代码这么少，一看就不是实现算法的地方*/
   
   
       protected int chooseRandomInt(int serverCount) {
           return ThreadLocalRandom.current().nextInt(serverCount);
       }
   
   	@Override
   	public Server choose(Object key) {
   		return choose(getLoadBalancer(), key);
   	}
   
   	@Override
   	public void initWithNiwsConfig(IClientConfig clientConfig) {//这个方法都是空的，说明没啥用
   
       }
   }
   ```

   > 对代码分析完之后，我们知道我们要修改的也就是中间的那几行，其余的没什么要改的

---

### 算法改造

来实现这样一个需求：

- A服务提供五次服务之后，假如服务B活着，跳转服务B
- B服务提供五次服务之后，假如服务C活着，跳转服务C
- C服务提供五次服务之后，假如服务A活着，跳转服务A



1. 第一步就是要把类名改了，改成我们自己的类名，比如`MyRule`

2. 改造算法

   ```java
   package com.bean.ribbonconfig;
   
   import com.netflix.client.config.IClientConfig;
   import com.netflix.loadbalancer.AbstractLoadBalancerRule;
   import com.netflix.loadbalancer.ILoadBalancer;
   import com.netflix.loadbalancer.Server;
   
   import java.util.List;
   import java.util.concurrent.ThreadLocalRandom;
   
   public class MyRule extends AbstractLoadBalancerRule {
   
   
       /*
           需求：
           - A服务提供五次服务之后，假如服务B活着，跳转服务B
           - B服务提供五次服务之后，假如服务C活着，跳转服务C
           - C服务提供五次服务之后，假如服务A活着，跳转服务A
       */
   
       private int total = 0;  //我们设置一下当前服务被调用的次数
       private int currentIndex = 0;  //设置一下现在在哪个服务
   
       /*这个代码这么多，一看就是实现算法的地方*/
       public Server choose(ILoadBalancer lb, Object key) {
           if (lb == null) {   //假如没有负载均衡，就返回null
               return null;
           }
           Server server = null;
   
           while (server == null) {    //当没有服务的时候
               if (Thread.interrupted()) {
                   return null;
               }
               List<Server> upList = lb.getReachableServers(); //获取还活着的服务
               List<Server> allList = lb.getAllServers();  //获取所有的服务
   
   
               /*-------------------------------------------------*/
               /*我们知道，这下面就是具体的算法，上边的都是前期的准备工作*/
   
   //            int serverCount = allList.size();
   //            if (serverCount == 0) {
   //                return null;
   //            }
   //
   //            int index = chooseRandomInt(serverCount);   //这个是索引，用于指向当前用哪个服务
   //            server = upList.get(index); //让服务 = 从活着的服务里面获取新的服务
   
               //2. 我们把上面的全部都注释掉
               if (total<5){   //假如当前服务的调用次数小于5次
                   server = upList.get(currentIndex);  //让服务为当前还活着的服务
                   /*
                       这里就是优化了，我完全可以让server = allList.get(currentIndex)，但是这样的话假如服务崩了就没治了
                    */
                   total++;    //让总数++
               }else {
                   total = 0;  //假如超过了5次，那么对总数进行重置
                   currentIndex++; //我们的游标也往前一位
                   if (currentIndex>=upList.size()){
                       /*
                           假如游标 >= 还活着的总个数
                           这里应该是游标 >= 还活着的总数量，不要忘记我们的游标是从0开始的
                        */
                       currentIndex = 0;   //重置游标
                   }
                   server = upList.get(currentIndex); //从活着的服务中，获取指定的服务来进行操作
               }
   
               /*---------------------------------------------*/
               /*上面的就是具体的算法，下面就是一些收尾*/
   
               if (server == null) {   //假如服务没有获取到，还为null
                   Thread.yield();     //进行处理，咋处理的也看不懂....
                   continue;
               }
   
               if (server.isAlive()) { //假如这个服务还活着
                   return (server);    //返回
               }
   
               server = null;  //服务置空
               Thread.yield(); //处理
           }
   
           return server;
   
       }
   
   
       /*下面这三个方法代码这么少，一看就不是实现算法的地方*/
   
   
       protected int chooseRandomInt(int serverCount) {
           return ThreadLocalRandom.current().nextInt(serverCount);
       }
   
   	@Override
   	public Server choose(Object key) {
   		return choose(getLoadBalancer(), key);
   	}
   
   	@Override
   	public void initWithNiwsConfig(IClientConfig clientConfig) {//这个方法都是空的，说明没啥用
   
       }
   }
   ```

   > 这个算法很low，但是毕竟也是改造了

3. 配置配置类`com.bean.ribbonconfig.RibbonConfig`

   > 按照官网说的，配置类要有`@Configuration`

   ```java
   @Configuration
   public class RibbonConfig {
       
       @Bean
       public IRule myRule(){
           return new MyRule();
       }
       
   }
   ```

4. 修改主启动类，让它扫描到具体的包

   ```java
   @SpringBootApplication
   @EnableEurekaClient //注解
   @RibbonClient(name = "SPRINGCLOUD-PROVIDER-DEPT",configuration = RibbonConfig.class)    
   //name指的是应用名称，也就是服务提供者的spring.application.name = springcloud-provider-dept
   public class DeptConsumer_80 {
       public static void main(String[] args) {
           SpringApplication.run(DeptConsumer_80.class,args);
       }
   }
   ```

5. 开启7001,8001,8002,8003,80，来测试一下

   > 成功

----

----

# Feign

## 简介

Feign是声明式的web service客户端，它让微服务之间的调用变得更简单了，类似conrtoller调用service。SpringCloud集成了Ribbon和Eureka，可在使用Feign时提供负载均衡的http客户端



只需要创建一个接口，然后添加注解即可!



feign主要是社区，大家都习惯面向接口编程，这个是很多开发人员的规范。调用微服务访问两种方法：

- 微服务名字(Ribbon)
- 接口和注释(Feign)



**Feign能干嘛**

- 旨在使编写Java Http客户端变得容易

- 前面在使用Ribbon+RestTemplate时，利用RestTemplate对Http请求的封装处理，形成了一套模板化的调用方法。

  但是在实际开发中，由于对服务依赖的调用可能不止一处，往往一个接口会被多粗调用

  所以通常都会针对每一个微服务自行封装一些客户端类来包装这些依赖服务的调用。

  所以，Feign在此基础上做了进一步的封装

  ==在Feign的实现下，我们只需要创建一个接口并加上注释来配置它(类似于以前Dao接口上标注Mapper注解)==

  现在是在一个微服务接口上标注一个Feign注解

> Feign也可以实现负载均衡，不过和Ribbon实现的方式不同，两者都可以用，两者都要会

## 使用步骤

要使用Feign，我们要进行改造

按理说我们应该对`springcloud-consumer-80`进行改造，但是随后一想，代码不能全丢了，所以我们新建一个模块，将80的所有代码全都移过去，然后对它进行改造



新建的模块叫做`springcloud-consumer-feign`，端口还是80，主启动类名字改为`FeignDeptConsumer_80`



1. 将Ribbon相关的全部代码删掉，包括`com.bean.ribbonconfig.*`，主启动类的注解配置

2. 因为我们是客户端服务，所以我们要在`springcloud-api`进行改造

   1. 导入Feign的依赖

      > Feign的依赖和Ribbin的依赖区别就是里面的ribbon改为feign，比如：
      >
      > ```xml
      >         <!-- Ribbon -->
      >         <dependency>
      >             <groupId>org.springframework.cloud</groupId>
      >             <artifactId>spring-cloud-starter-ribbon</artifactId>
      >             <version>1.4.6.RELEASE</version>
      >         </dependency>
      > ```
      >
      > ```xml
      >         <!-- Feign -->
      >         <dependency>
      >             <groupId>org.springframework.cloud</groupId>
      >             <artifactId>spring-cloud-starter-feign</artifactId>
      >             <version>1.4.6.RELEASE</version>
      >         </dependency>
      > ```
      >
      > 

   2. 添加一个service包，里面加上服务，服务里面的方法都是服务提供者里面的方法，没有任何区别

      - `com.bean.service.DeptClientService`

      ```java
      @FeignClient(value = "SPRINGCLOUD-PROVIDER-DEPT")//value:微服务的名字，也就是提供者的spring.application.name = springcloud-provider-dept
      public interface DeptClientService {
      
          @PostMapping("/dept/add")
          public boolean addDept(Dept dept);
      
          @GetMapping("/dept/get/{id}")
          public Dept queryById(@PathVariable("id")Long id);
      
          @GetMapping("/dept/list")
          public List<Dept> queryAll();
      
      }
      ```
      
> 注意加入的注解
      >
      > 我们以前在controller中写的东西全都移动到这里来了，而且请求的路径也一样
      >

3. 修改`springcloud-consumer-feign`注意先导入pom依赖

   1. 修改conrtoller

         ```java
         @RestController
         public class DeptConsumerController {
         
         //    @Autowired
         //    private RestTemplate restTemplate;
         //    RestTemplate也不要了
             @Autowired
             private DeptClientService service = null;
         
         
         //    private static final String REST_URL_PREFIX="http://SPRINGCLOUD-PROVIDER-DEPT";
             //这个我们不要了，因为我们要使用接口方式去调用
         
             @RequestMapping("/consumer/dept/get/{id}")
             public Dept get(@PathVariable("id")Long id){
         //        return restTemplate.getForObject(REST_URL_PREFIX+"/dept/get"+id,Dept.class);
                 //这个我们也不要了
                 return this.service.queryById(id);
             }
         
             @RequestMapping("/consumer/dept/add")
             public boolean add(Dept dept){
         //        return restTemplate.postForObject(REST_URL_PREFIX+"/dept/add",dept,Dept.class);
                 return this.service.addDept(dept);
             }
         
             @RequestMapping("/consumer/dept/list")
             public List<Dept> list(){
         //        return restTemplate.getForObject(REST_URL_PREFIX+"/dept/list",List.class);
                 return this.service.queryAll();
             }
         
         }
         ```

   2. 修改主启动类

      ```java
      @SpringBootApplication
      @EnableEurekaClient
      @EnableFeignClients(basePackages = "com.bean.springcloud")//扫描哪个地方的包
      public class FeignDeptConsumer_80 {
          public static void main(String[] args) {
              SpringApplication.run(FeignDeptConsumer_80.class,args);
          }
      }
      ```

      > 视频上还要加上一个@Component注解，但是其实不用，因为
      >
      > @SpringBootApplication内包含@Componnet注解
      >
      > 再加上@Component注解会报错
      >
      > 不知道视频上为什么没有报错

4. 测试

   > 成功
   >
   > 关于Ribbon和Feign，其实用哪个都可以
   
5. 假如报错：

   > 1. ==com.netflix.client.ClientException: Load balancer does not have available server for client==，可以在`feign-80`配置文件上加入：
   >
   >  ```yaml
   > ribbon:
   >   eureka:
   >     enabled: true
   >  ```
   >
   > 2. 等待一会就好

   

----

---



# Hystrix（豪猪）

## 服务熔断

### 理论准备



**分布式系统面临的问题**

复杂分布式体系结构中的应用程序有数十个依赖关系，每一个依赖在某些时候将不可避免的失败



**服务雪崩**



​		多个微服务之间调用的时候，假设微服务A调用微服务B和微服务C,微服务B和微服务C又调用其他的微服务,
这就是所谓的“扇出”、如果扇出的链路上某个微服务的调用响应时间过长或者不可用，对微服务A的调用就会占用
越来越多的系统资源，进而引起系统崩溃,所谓的“雪崩效应”。

​		对于高流量的应用来说，单- -的后端依赖可能会导致所有服务器上的所有资源都在几秒中内饱和。比失败更糟
糕的是，这些应用程序还可能导致服务之间的延迟增加，备份队列,线程和其他系统资源紧张，导致整个系统发生
更多的级联故障，这些都表示需要对故障和延迟进行隔离和管理，以便单个依赖关系的失败，不能取消整个应用程
序或系统。

我们需要：弃车保帅

![img](SpringCloud.assets/soa-2-640.png)

> 比如这个时候，A调用H，H调用I，I调用P
>
> 假如某一个服务调用了别的服务的时候调用失败，比如H调用I失败
>
> 那么服务雪崩
>
> 因为这个时候服务崩了，这个时候我们使用I的备份CFLOR等等，只对客户端返回一个消息：服务崩了



**什么是Hystrix**

​		Hystrix是一个用于处理分布式系统的延迟和容错的开源库, 在分布式系统里，许多依赖不可避免的会调用失
败，比如超时，异常等，Hystrix能够保证在一个依赖出问题的情况下， 不会导致整体服务失败，避免级联故障,
以提高分布式系统的弹性。

​		“断路器”本身是一种开关装置, 当某个服务 单元发生故障之后，通过断路器的故障监控(类似熔断保险丝)，==向
调用方返回一个服务预期的，可处理的备选响应(FallBack) ，而不是长时间的等待或者抛出调用方法无法处理
的异常，这样就可以保证了服务调用方的线程不会被长时间==，不必要的占用，从而避免了故障在分布式系统中的蔓
延,乃至雪崩



**Hystrix能干嘛**

- 服务降级
- 服务熔断
- 服务限流
- 服务监控



**服务熔断是什么**



​		熔断机制是对应雪崩效应的一-种微服务链路保护机制。

​		当扇出链路的某个微服务不可用或者响应时间太长时，会进行服务的降级,==进而熔断该节点微服务的调用，快
速返回错误的响应信息==。当检测到该节点微服务调用响应正常后恢复调用链路。在SpringCloud框架里熔断机制通
过Hystrix实现。Hystrix会监控微服务间调用的状况， 当失败的调用到一定阈值，缺省是5秒内20次调用失败就会
启动熔断机制。

​		熔断机制的注解是`@HystrixCommand`.



### 前期准备、熔断编写及测试

1. 把8001的项目拷贝成一个一抹一样的模块，叫做`springcloud-provider-dept-hystrix-8001`

2. 把启动类改一下，把application中的描述信息改一下

   ```java
   @SpringBootApplication
   @EnableEurekaClient
   @EnableDiscoveryClient
   public class HystrixDeptProvider_8001 {
   
       public static void main(String[] args) {
           SpringApplication.run(HystrixDeptProvider_8001.class,args);
       }
   }
   ```

   ```yaml
   
   server:
     port: 8001
   
   mybatis:
     type-aliases-package: com.bean.pojo
     mapper-locations: classpath:mybatis/mapper/*.xml
     config-location: classpath:mybatis/mybatis-config.xml
   
   spring:
     application:
       name: springcloud-provider-dept
     datasource:
       type: com.alibaba.druid.pool.DruidDataSource
       driver-class-name: org.gjt.mm.mysql.Driver
       url: jdbc:mysql://localhost:3306/db01?useUnicode=true&characterEncoding=utf-8&serverTimezone=Asia/Shanghai
       username: root
       password: root
   
   # Eureka生产者端的配置
   eureka:
     client:
       service-url:
         defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/
     instance:
       instance-id: springcloud-provider-hystirx_dept8001 # 描述改一下
   
   info:
     app.name: bean-project
     company.name: blog.bean
   ```

3. 导入依赖，依赖类似ribbon改为feign，也是把stater后面的字改为hystrix

   ```xml
           <dependency>
               <groupId>org.springframework.cloud</groupId>
               <artifactId>spring-cloud-starter-hystrix</artifactId>
               <version>1.4.6.RELEASE</version>
           </dependency>
   ```

   > 前面的pom文件都是复制8001的，这里只放出hystrix的

4. 重新写一下Controller，让Controller结构更加清楚，添加熔断机制

   ```java
   /*把没用的Conctroller都删了*/
   @RestController
   public class DeptConrtoller {
   
       @Autowired
       private DeptService deptService;
   
       @GetMapping("/dept/get/{id}")
       @HystrixCommand(fallbackMethod = "hystrixGet")//假如服务崩了，会自动调用hystrixGet方法
       public Dept get(@PathVariable("id")Long id){
           Dept dept = deptService.queryById(id);
           /*
               这里有点问题：假如查找不存在的id，那么查出来就是null
               这么返回前端肯定爆出500
               那么这个时候就需要服务熔断
            */
           if (dept==null){
               //假如dept为null，那么直接抛出异常，模拟服务崩溃
               throw new RuntimeException();
           }
           
           return dept;
       }
       
       //服务熔断机制，服务崩了之后的备选方案
       public Dept hystrixGet(@PathVariable("id")Long id){
           return new Dept()
                       .setDname("id=>"+id+" 不存在，或者无法查询")
                       .setDb_source("no this database in MySQL");
       }
   
   
   
   }
   ```

5. 在启动类上添加对熔断的支持

   ```java
   @SpringBootApplication
   @EnableEurekaClient
   @EnableDiscoveryClient
   @EnableCircuitBreaker//注意不是这个@EnableHystrix，这个注解是另一个注解，我们要用这个注解，把断路器打开
   public class HystrixDeptProvider_8001 {
   
       public static void main(String[] args) {
           SpringApplication.run(HystrixDeptProvider_8001.class,args);
       }
   }
   
   ```

6. 启动7001，hystrix-8001，feign-80并测试

   成功

   ![1585285417123](SpringCloud.assets/1585285417123.png)

-----

## 服务降级

资源不够用了，忍痛把一些服务关掉



其实熔断和降级其实是差不多的，区别之一就是服务熔断是针对某一个方法，而服务降级是针对某一个类



我们对`springcloud-api`进行操作



1. 新建类`DeptClientServiceFallbackFactory`，继承`FallbackFactory`

   ```java
   @Component
   public class DeptClientServiceFallbackFactory implements FallbackFactory {
       @Override
       public Object create(Throwable throwable) {
           return null;
       }
   }
   ```

2. 对这个类进行改造

   ```java
   @Component
   public class DeptClientServiceFallbackFactory implements FallbackFactory {
       @Override
       public DeptClientService create(Throwable throwable) {
           //返回值为DeptClientService这个类，也就是说对这个类进行服务降级
   
           return new DeptClientService() {
               @Override
               public boolean addDept(Dept dept) {
                   return false;
               }
   
               @Override
               public Dept queryById(Long id) {
                   return new Dept()
                           .setDname("进行了服务降级")
                           .setDb_source("no this database in MySQL");
               }
   
               @Override
               public List<Dept> queryAll() {
                   return null;
               }
           };
       }
   }
   
   ```

   > 为了看的清楚，这里只填写一个方法

3. 对`DeptClientService改造`

   ```java
   //value:微服务的名字，也就是提供者的spring.application.name = springcloud-provider-dept
   //fallbackFactory：进行服务降级的类
   @FeignClient(value = "SPRINGCLOUD-PROVIDER-DEPT",fallbackFactory = DeptClientServiceFallbackFactory.class)
   public interface DeptClientService {
   
       @PostMapping("/dept/add")
       public boolean addDept(Dept dept);
   
       @GetMapping("/dept/get/{id}")
       public Dept queryById(@PathVariable("id")Long id);
   
       @GetMapping("/dept/list")
       public List<Dept> queryAll();
   
   }
   ```

4. 对`springcloud-consumer-dept-feign`中的`application.yaml`进行改造

   ```yaml
   server:
     port: 80
   
   #Eureka配置
   eureka:
     client:
       register-with-eureka: false # 我们不向注册中心注册自己，因为是消费者不是提供者
       service-url:
         defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/
   ribbon:
     eureka:
       enabled: true
   #feign-hystrix 服务降级
   feign:
     hystrix:
       enabled: true
   
   ```

   

5. 启动7001,`hystirx-8001`,`feign-80`进行测试

   > 一开始我们可以访问（正常情况下）
   >
   > ![1585296374799](SpringCloud.assets/1585296374799.png)
   >
   > 后来突发情况（关闭8001），出现了服务降级的提示
   >
   > ![1585296395096](SpringCloud.assets/1585296395096.png)

-----

----

## 服务熔断和服务降级的联系和区别

**服务熔断**

在服务端，某个服务超时或者异常，引起熔断



**服务降级**

在客户端，从整体网站的负载进行考虑，当某个服务熔断或者关闭之后，服务将不再进行调出

此时在客户端我们准备一个FallbackFactory，返回一个缺省值，整体服务水平下降了，但是好歹能用，比直接挂掉强

----

## 服务监控

**Dashboard**服务监控

这个服务监控与消费者相关，与服务端无关了

### 环境搭建

1. 新建一个模块`springcloud-consumer-hystrix-dashboard`

2. 从80导入依赖，并且增加`hystrix`的依赖，增加`hystrix-dashboard`的依赖

   > `hystrix`依赖就是将ribbon的依赖中的ribbon字符串改为hystrix
   >
   > `hystrix-dashboard`依赖就是hystrix依赖中的hystrix改为hystrix-dashboard

   ```yaml
           <!--Hystrix-->
           <dependency>
               <groupId>org.springframework.cloud</groupId>
               <artifactId>spring-cloud-starter-hystrix</artifactId>
               <version>1.4.6.RELEASE</version>
           </dependency>
           <!-- hystrix-dashboard -->
           <dependency>
               <groupId>org.springframework.cloud</groupId>
               <artifactId>spring-cloud-starter-hystrix-dashboard</artifactId>
               <version>1.4.6.RELEASE</version>
           </dependency>
   ```

3. 改一下端口为`9001`

4. 新建包`com.bean`，新建启动类，使用注解`@EnableHystrixDashboard`

   ```java
   @SpringBootApplication
   @EnableHystrixDashboard
   public class HystrixDashboard {
       public static void main(String[] args) {
           SpringApplication.run(HystrixDashboard.class,args);
       }
   }
   ```

5. 检查一遍服务端是否有监控的依赖

   > 分别去`8001`，`8002`，`8003`查看是否有`spring-boot-starter-actuator`依赖，没有就加上
   >
   > ```xml
   >         <!--监控信息-->
   >         <dependency>
   >             <groupId>org.springframework.boot</groupId>
   >             <artifactId>spring-boot-starter-actuator</artifactId>
   >         </dependency>
   > ```

6. 启动9001，尝试访问`localhost:9001/hystrix`

   ![1585298635671](SpringCloud.assets/1585298635671.png)

   > 这个页面有：
   >
   > 1. 监控的地址
   > 2. 轮询事件
   > 3. 监控实例的名字
   > 4. 要配置一个`http://hystrix-app:port/actuator/hystrix.stream`

---

### 使用



假如我们想注册`hystrix-8001`，那么需要在启动类上增加一个Bean对象

1. 首先将`hystrix`的包导入`hystrix-8001 `，不过经过之前的编写应该已经有了

2. 在启动类上加上新的Bean对象

   ```java
   @SpringBootApplication
   @EnableEurekaClient
   @EnableDiscoveryClient
   @EnableCircuitBreaker
   public class HystrixDeptProvider_8001 {
   
       public static void main(String[] args) {
           SpringApplication.run(HystrixDeptProvider_8001.class,args);
       }
   
       @Bean
       public ServletRegistrationBean registrationBean(){
           ServletRegistrationBean registrationBean = new ServletRegistrationBean(new HystrixMetricsStreamServlet());
           registrationBean.addUrlMappings("/actuator/hystrix.stream");    
           //这个地方，我们访问localhost:9001/hystrix的时候已经提示过了
           return registrationBean;
       }
   }
   ```

3. 开启`hystrix-dashboard-9001`,`7001`,`hystrix-8001`

   > 按照图中的分别填入
   >
   > ![1585299808522](SpringCloud.assets/1585299808522.png)
   >
   > > 注意，我们监控的是8001服务的地址:`http://localhost:8001/actuator/hystrix.stream`
   >
   > 点击按钮跳转 
   >
   > ![1585299819356](SpringCloud.assets/1585299819356.png)

4. 监控不到：

   > 1. 首先要监控的服务要启用熔断`@EnableCircuitBreaker`，我们启动的就是`hystrix-8001`这个带熔断的
   >
   > 2. 注意要加上Bean对象
   >
   >    ```java
   >    @Bean
   >    public ServletRegistrationBean registrationBean(){
   >        ServletRegistrationBean registrationBean = new ServletRegistrationBean(new HystrixMetricsStreamServlet());
   >        registrationBean.addUrlMappings("/actuator/hystrix.stream");    
   >    	//这个地方，我们访问localhost:9001/hystrix的时候已经提示过了
   >        return registrationBean;
   >    }
   >    ```
   >
   >    
   >
   > 3. 该服务只能监控有熔断注解`@HystrixCommand`的方法



监控中所代表的意义：

**七色**

![1585300209487](SpringCloud.assets/1585300209487.png)

**一圈**

实心圆：两种含义，通过颜色的变化代表着实例的健康程度，通过大小代表流量变化

1. 绿————黄————橙————红	递减（虽然看着像灰色，但确实是绿色）
2. 流量越大，实心圆越大

![1585300087172](SpringCloud.assets/1585300087172.png)

---

----

# Zuul



## 概述

`http://localhost:9001/hystrix` ，其中`localhost:9001`不应该被显示出来，而是应该使用`www.xxx`来代替

但是这样出来了一个问题：不同的微服务有不同的地址，比如8001,8002,8003等等

我们要配置的这个路径是一个路径，那么也需要分发的概念

**什么是Zuul?** 

​		Zuul包含了对请求的路由和过滤两个最主要的功能:
​		

​		其中路由功能负责将外部请求转发到具体的微服务实例上，是实现外部访问统一入口的基础， 而过滤器功能则
负责对请求的处理过程进行干预，是实现请求校验,服务聚合等功能的基础。

​		ZuuI和Eureka进行整合,==将Zuul自身注册为Eureka服务治理下的应用(Zuul也要注册到Eureka中)==，同时从Eureka中获得其他微服务的消息，也即以后的访问微服务都是通过Zuul跳转后获得。



​		注意: Zuul服务最终还是会注册进Eureka

​		提供:代理+路由+过滤三大功能!





**Zuul能干嘛?**

- 路由

- 过滤

  

官网文档: https://github.com/Netflix/zuul

----

## 环境搭建

1. 新建项目`springcloud-zuul-9527`

2. 导入依赖，从`springcloud-consumer-hystrix-dashboard`中导入依赖

3. 新增依赖`zuul`，也是像之前一样，把hystrix的依赖的hystrix字符串改为zuul

   ```xml
           <!--zuul-->
           <dependency>
               <groupId>org.springframework.cloud</groupId>
               <artifactId>spring-cloud-starter-zuul</artifactId>
               <version>1.4.6.RELEASE</version>
           </dependency>
   ```

   

4. 配置`application.yaml`，设置端口为9527，设置服务名称为`springcloud-zuul`，设置eureka，信息,zuul

   ```yaml
   server:
     port: 9527
   
   #设置服务名字
   spring:
     application:
       name: springcloud-zuul
   
   
   # Eureka生产者端的配置
   eureka:
     client:
       service-url:
         defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/
         # 注册中心地址把7001,7002,7003都加上
     instance:
       instance-id: springcloud-zuul # 描述信息
   
   
   # 配置信息
   info:
     app.name: bean-project
     company.name: blog.bean
   
   zuul:
     routes:
       #zuul.routes是一个map类型的，所以我们下面的随便起名字就好，这个map的用法是将所有用到的服务名字都用设定的路径来代替
       dept.serviceId: springcloud-provider-dept # 这个配置的是服务名字
       dept.path: /dept/** # 我们使用/dept/**来代替
   
   ```

   

5. 更改host文件，新增一个本地域名为`www.zuul.com`

   ![1585301795800](SpringCloud.assets/1585301795800.png)

6. 写一个包`com.bean.springcloud`，新增启动类，增加注解`@EnableZuulProxy`

   ```java
   @SpringBootApplication
   @EnableZuulProxy
   public class ZuulProxy {
       public static void main(String[] args) {
           SpringApplication.run(ZuulProxy.class,args);
       }
   }
   
   ```

7. 启动`7001`,`hystrix-8001`,`9527`测试 

   ![1585301995089](SpringCloud.assets/1585301995089.png)



但是这样有一个问题，这样访问之后直接输入`xxx/项目名字/xxx`也能访问：

`http://www.zuul.com:9527/springcloud-provider-dept/dept/get/1`



我们要配置让原路径不能访问：

```yaml
server:
  port: 9527

#设置服务名字
spring:
  application:
    name: springcloud-zuul


# Eureka生产者端的配置
eureka:
  client:
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/
  instance:
    instance-id: springcloud-zuul # 描述信息


# 配置信息
info:
  app.name: bean-project
  company.name: blog.bean

zuul:
  routes:
    dept.serviceId: springcloud-provider-dept
    dept.path: /mydept/**
  ignored-services: "*"# 忽略这个服务下的原路径，单个服务可以使用服务名，比如：springcloud-provider-dept
```

![1585303019255](SpringCloud.assets/1585303019255.png)

> 果然不能访问了，项目名字也隐藏了

![1585303045504](SpringCloud.assets/1585303045504.png)

> 现在只能用这个路径来进行访问





**配置前缀**

配置前缀之后首先要写这个前缀，然后是项目路径

```yaml
server:
  port: 9527

#设置服务名字
spring:
  application:
    name: springcloud-zuul


# Eureka生产者端的配置
eureka:
  client:
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/
  instance:
    instance-id: springcloud-zuul # 描述信息


# 配置信息
info:
  app.name: bean-project
  company.name: blog.bean

zuul:
  routes:
    dept.serviceId: springcloud-provider-dept
    dept.path: /mydept/**
  ignored-services: "*"
  prefix: /bean # 配置前缀
```

![1585303260328](SpringCloud.assets/1585303260328.png)

-----

-----

# SpringCloud Config

微服务A，B，C有三个配置文件

假如我们能有一个具体的配置中心，要什么配置就去配置中心读，那么就好了



Spring Cloud Config为微服务架构中的微服务提供集中化的外部配置支持，配置服务器为各个不同微服务应用的所有环节都提供了一个中心化的外部配置



SpringCloudConfig分为服务端和客户端两部分



**服务端**

也称分布式配置中心，它是一个独立的微服务应用，用来连接配置服务器并为客户端提供获取配置信息，加密，解密信息等访问接口



**客户端**

通过指定的配置中心来管理应用资源，以及与业务相关的配置内容，并且在启动的时候从配置中心获取和加载配置信息。

配置服务器默认采用git来存储配置信息，这样就有助于对环境配置进行版本管理。

并且可以通过Git客户端工具来方便地进行管理和访问配置内容



下面这张图很清晰了：用户链接服务，服务链接远程仓库

![1585308463188](SpringCloud.assets/1585308463188.png)



**SpringCloud config分布式配置中心能干嘛**

- 集中管理配置文件
- 不同环境，不同配置，动态化的配置更新，分环境部署,比如/dev /test/ /prod /beta /release -
- 运行期间动态调整配置，不再需要在每个服务部署的机器上编写配置文件,服务会向配置中心统-拉取配置自
  己的信息。
- 当配置发生变动时，服务不需要重启，即可感知到配置的变化，并应用新的配置
- 将配置信息以RIEST接口的形式暴露



**SpringCloud config分布式配置中心与github整合**

由于Spring Cloud Config默认使用Git来存储配置文件(也有其他方式， 比如支持SVN和本地文件)

但是最推荐的还是Git，而且使用的是http / https访问的形式;



还可以使用码云，coding，建议使用码云

---

## 服务端



在码云上新建一个仓库springcloudConfig，然后往里面添加一个配置文件`application.yaml`

```yaml
spring:
 profiles:
  active: dev

---

spring:
 profiles: dev
 application:
  name: springcloud-cofnig-dev

---
spring:
 profiles: test
 application:
  name: springcloud-cofnig-test
```



关于Git的不说，下面说一下服务



新建一个模块`springcloud-config-server-3344`，在这里面配置



1. 导入依赖

   ```xml
       <dependencies>
           <!--Web-->
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-web</artifactId>
           </dependency>
   
           <!--ConfigServer-->
           <!-- https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-config-server -->
           <dependency>
               <groupId>org.springframework.cloud</groupId>
               <artifactId>spring-cloud-config-server</artifactId>
               <version>2.1.6.RELEASE</version>
           </dependency>
   
       </dependencies>
   ```

   > 一定要有一个springcloudconfigserver，其余的有需求就再加

2. `application.yaml`

   ```yaml
   server:
     port: 3344
   spring:
     application:
       name: springcloud-config-server
     cloud:  # 链接远程仓库
       config:
         server:
           git:
             uri: https://gitee.com/BEANGITEE/springcloudConfig.git
             # 具体的位置，比如码云，为此我单独创建了一个仓库，用https的
   ```

3. 主启动类开启，加上那个注解

   ```java
   @SpringBootApplication
   @EnableConfigServer //加上注解
   public class Config_Server_3344 {
       public static void main(String[] args) {
           SpringApplication.run(Config_Server_3344.class,args);
       }
   }
   ```

4. 测试

   **后缀版**

   ![1585305839425](SpringCloud.assets/1585305839425.png)

   **无后缀版**

   ![1585306025200](SpringCloud.assets/1585306025200.png)

----

## 客户端

在git文件中新建一个`config-client.yaml`作为客户端的配置

```yaml
spring:
 profiles:
  active: dev

---
server:
  port: 8201
  
spring:
 profiles: dev
 application:
  name: springcloud-cofnig-dev

# Eureka配置
eureka:
  client:
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka/

---
server:
  port: 8202
  
spring:
 profiles: test
 application:
  name: springcloud-cofnig-test

# Eureka配置
eureka:
  client:
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka/
```



1. 写一个模块：`springcloud-config-client-3355`

2. 依赖

   ```xml
       <dependencies>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-web</artifactId>
           </dependency>
           <!--config，这个是客户端的config-->
           <dependency>
               <groupId>org.springframework.cloud</groupId>
               <artifactId>spring-cloud-starter-config</artifactId>
               <version>2.1.1.RELEASE</version>
           </dependency>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-actuator</artifactId>
           </dependency>
       </dependencies>
   ```

3. 编写配置`bootstrap.yaml`

   > 以前学过`application.yaml`，这里使用`bootstrap.yaml`
   >
   > `bootstrap.yaml`是系统级别的配置，而`application.yaml`是用户级别的配置
   >
   > 一般情况下使用用户级别的够了，但是这里有可能会和远程的冲突，所以使用系统级别的加载器，更高优先级

   ```yaml
   spring:
     cloud:
       config:
         uri: http://localhost:3344
         # 这里连接的是我们刚才编写服务端的地址，而服务端链接远程仓库。这就是用户连接服务，服务连接远程仓库
         name: config-client #要从git上读取的资源，不用后缀
         profile: dev #使用dev开发环境
         label: master # 去master分支拿
   ```

4. 编写Conrtoller

   ```java
   @RestController
   public class ConfigController {
   
       @Value("${spring.application.name}")
       private String applicationName;
       @Value("${eureka.client.service-url.defaultZone}")
       private String eurekaServer;
       @Value("${server.port}")
       private String port;
   
       @RequestMapping("/config")
       public String getConfig(){
           return "applicationName："+applicationName+"，eurekaServer："+eurekaServer+"，port："+port;
       }
   
   
   }
   ```

5. 编写启动类

   ```java
   @SpringBootApplication
   public class ConfigClient_3355 {
       public static void main(String[] args) {
           SpringApplication.run(ConfigClient_3355.class,args);
       }
   }
   ```

6. 启动服务端，然后启动客户端

   因为我们在远程的配置上写的是8201，所以读取到的端口号自动配置成了8201，现在我们进行访问：

   ![1585309209522](SpringCloud.assets/1585309209522.png)

   > 成功

-----

从此之后别人看不懂了你的配置，实现了配置与编码解耦

