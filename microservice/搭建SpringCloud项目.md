# 项目搭建

## 版本说明

> [官网说明](https://github.com/alibaba/spring-cloud-alibaba/wiki/%E7%89%88%E6%9C%AC%E8%AF%B4%E6%98%8E )

### 2022.x 分支

适配 Spring Boot 3.0，Spring Cloud 2022.x 版本及以上的 Spring Cloud Alibaba 版本按从新到旧排列如下表（最新版本用*标记）： *(注意，该分支 Spring Cloud Alibaba 版本命名方式进行了调整，未来将对应 Spring Cloud 版本，前三位为 Spring Cloud 版本，最后一位为扩展版本，比如适配 Spring Cloud 2022.0.0 版本对应的 Spring Cloud Alibaba 第一个版本为：2022.0.0.0，第个二版本为：2022.0.0.1，依此类推)*

| Spring Cloud Alibaba Version | Spring Cloud Version  | Spring Boot Version |
| ---------------------------- | --------------------- | ------------------- |
| 2022.0.0.0-RC2*              | Spring Cloud 2022.0.0 | 3.0.2               |
| 2022.0.0.0-RC1               | Spring Cloud 2022.0.0 | 3.0.0               |

### 2021.x 分支

适配 Spring Boot 2.4，Spring Cloud 2021.x 版本及以上的 Spring Cloud Alibaba 版本按从新到旧排列如下表（最新版本用*标记）：

| Spring Cloud Alibaba Version | Spring Cloud Version  | Spring Boot Version |
| ---------------------------- | --------------------- | ------------------- |
| 2021.0.5.0*                  | Spring Cloud 2021.0.5 | 2.6.13              |
| 2021.0.4.0                   | Spring Cloud 2021.0.4 | 2.6.11              |
| 2021.0.1.0                   | Spring Cloud 2021.0.1 | 2.6.3               |
| 2021.1                       | Spring Cloud 2020.0.1 | 2.4.2               |

### 2.2.x 分支

适配 Spring Boot 为 2.4，Spring Cloud Hoxton 版本及以下的 Spring Cloud Alibaba 版本按从新到旧排列如下表（最新版本用*标记）：

| Spring Cloud Alibaba Version      | Spring Cloud Version        | Spring Boot Version |
| --------------------------------- | --------------------------- | ------------------- |
| 2.2.10-RC1*                       | Spring Cloud Hoxton.SR12    | 2.3.12.RELEASE      |
| 2.2.9.RELEASE                     | Spring Cloud Hoxton.SR12    | 2.3.12.RELEASE      |
| 2.2.8.RELEASE                     | Spring Cloud Hoxton.SR12    | 2.3.12.RELEASE      |
| 2.2.7.RELEASE                     | Spring Cloud Hoxton.SR12    | 2.3.12.RELEASE      |
| 2.2.6.RELEASE                     | Spring Cloud Hoxton.SR9     | 2.3.2.RELEASE       |
| 2.2.1.RELEASE                     | Spring Cloud Hoxton.SR3     | 2.2.5.RELEASE       |
| 2.2.0.RELEASE                     | Spring Cloud Hoxton.RELEASE | 2.2.X.RELEASE       |
| 2.1.4.RELEASE                     | Spring Cloud Greenwich.SR6  | 2.1.13.RELEASE      |
| 2.1.2.RELEASE                     | Spring Cloud Greenwich      | 2.1.X.RELEASE       |
| 2.0.4.RELEASE(停止维护，建议升级) | Spring Cloud Finchley       | 2.0.X.RELEASE       |
| 1.5.1.RELEASE(停止维护，建议升级) | Spring Cloud Edgware        | 1.5.X.RELEASE       |

## 父工程

`spring-cloud-alibaba`介绍了如何快速搭建项目，地址为：[doc](https://spring-cloud-alibaba-group.github.io/github-pages/hoxton/en-us/index.html)



# simple 客户端

## web服务，注册中心，发现中心

### 项目结构

```
---microservice-parent
----server
-----system-server
```



### 父工程 pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.xieq</groupId>
    <artifactId>microservice-parent</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>pom</packaging>

    <modules>
        <module>server</module>
    </modules>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.3.12.RELEASE</version>
    </parent>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        <java.version>1.8</java.version>
        <spring.boot.version>2.3.12.RELEASE</spring.boot.version>
        <spring.cloud.version>Hoxton.SR12</spring.cloud.version>
        <spring.cloud.alibaba.version>2.2.9.RELEASE</spring.cloud.alibaba.version>
    </properties>


    <dependencyManagement>
        <dependencies>
            <!-- spring-cloud -->
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>${spring.cloud.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>

            <dependency>
                <groupId>com.alibaba.cloud</groupId>
                <artifactId>spring-cloud-alibaba-dependencies</artifactId>
                <version>${spring.cloud.alibaba.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
  
</project>
```



### 子项目pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.xieq</groupId>
        <artifactId>microservice-parent</artifactId>
        <version>1.0-SNAPSHOT</version>
    </parent>

    <artifactId>server</artifactId>
    <packaging>pom</packaging>
    <modules>
        <module>system-server</module>
    </modules>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>


    <dependencies>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
        </dependency>

        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
        </dependency>
    </dependencies>
</project>
```



### 项目启动类 SystemApplication.java

```java
package org.xieq.system;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

/**
 * @author xieq
 * @date 2023/6/20 14:51
 */
@SpringBootApplication
public class SystemApplication {
    public static void main(String[] args) {
        SpringApplication.run(SystemApplication.class, args);
    }
}

```



### 配置文件 bootstrap.yml

> 这里需要注意的时spring.application.name 和server-addr是必须的。
>
> 项目启动的时候会去nacos查找配置文件。
>
> 而且配置文件名需要bootstrap.yml,如果是application.yml则file-extension无效，暂时不知什么原因。

```yaml
spring:
  application:
    name: system
  cloud:
    nacos:
      config:
        server-addr: localhost:8848
        file-extension: yaml
```

在 Nacos Spring Cloud 中，`dataId` 的完整格式如下：

```plain
${prefix}-${spring.profiles.active}.${file-extension}
```

- `prefix` 默认为 `spring.application.name` 的值，也可以通过配置项 `spring.cloud.nacos.config.prefix`来配置。
- `spring.profiles.active` 即为当前环境对应的 profile，详情可以参考 [Spring Boot文档](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-profiles.html#boot-features-profiles)。 **注意：当 `spring.profiles.active` 为空时，对应的连接符 `-` 也将不存在，dataId 的拼接格式变成 `${prefix}.${file-extension}`**
- `file-exetension` 为配置内容的数据格式，可以通过配置项 `spring.cloud.nacos.config.file-extension` 来配置。目前只支持 `properties` 和 `yaml` 类型。

参考地址：https://nacos.io/zh-cn/docs/v2/ecology/use-nacos-with-spring-cloud.html



### nacos配置文件system.yaml

> 默认system.properties，我上面配置了file-extension:yaml，所以为system.yaml

```yaml
server:
  port: 8001
spring:
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848
message: 李四
```



## gateway

### 项目结构

```
---microservice-parent
----gateway
----server
-----system-server
```

### pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.xieq</groupId>
        <artifactId>microservice-parent</artifactId>
        <version>1.0-SNAPSHOT</version>
    </parent>

    <artifactId>gateway</artifactId>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

    <dependencies>

        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-gateway</artifactId>
        </dependency>

        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
        </dependency>

        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
        </dependency>

    </dependencies>

</project>
```



### 启动类

```java
package org.xieq.gateway;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

/**
 * @author xieq
 * @date 2023/6/20 16:57
 */
@SpringBootApplication
public class GatewayApplication {
    public static void main(String[] args) {
        SpringApplication.run(GatewayApplication.class, args);
    }
}

```



### bootstrap.yml

```
spring:
  application:
    name: gateway
  cloud:
    nacos:
      config:
        server-addr: localhost:8848
        file-extension: yaml
```



### nacos配置文件

```yaml
server:
  port: 8000
spring:
  cloud:
    gateway:
      discovery:
        locator:
          enabled: true
      routes:
        - id: system
          uri: lb://system
          predicates: # 断言(就是路由转发要满足的条件)
            - Path=/system/**
```



# 创建一个公共的api

## 项目结构

```
---microservice-parent
----api
-----system-api
----server
-----system-server
```

## pom

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.xieq</groupId>
        <artifactId>microservice-parent</artifactId>
        <version>1.0-SNAPSHOT</version>
    </parent>

    <artifactId>api</artifactId>
    <packaging>pom</packaging>
    <modules>
        <module>system-api</module>
    </modules>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>


    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-openfeign</artifactId>
            <scope>provided</scope>
        </dependency>
    </dependencies>

</project>
```



# 创建一个新的服务

## 项目结构

```
---microservice-parent
----api
-----system-api
----server
-----system-server
-----seckill-server
```



## Pom 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.xieq</groupId>
        <artifactId>server</artifactId>
        <version>1.0-SNAPSHOT</version>
    </parent>

    <artifactId>seckill-server</artifactId>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>


    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-openfeign</artifactId>
        </dependency>

        <dependency>
            <groupId>org.xieq</groupId>
            <artifactId>system-api</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
    </dependencies>
</project>
```



## 配置文件 bootstrap.yml

```yaml
spring:
  application:
    name: seckill
  cloud:
    nacos:
      config:
        server-addr: localhost:8848
        file-extension: yaml
```

## nacos配置 seckill.yml

```yml
server:
  port: 8003
```

## 启动类 SeckillApplication

> 这里注意EnableFeignClients
>
> 所有的api接口必须通过这个里配置基础包
>
> ComponentScan里无效

```java
package org.xieq.seckill;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.openfeign.EnableFeignClients;

/**
 * @author xieq
 * @date 2023/6/21 14:25
 */
@SpringBootApplication
@EnableFeignClients(basePackages = "org.xieq.system.api")
public class SeckillApplication {
    public static void main(String[] args) {
        SpringApplication.run(SeckillApplication.class, args);
    }
}
```

## 控制层测试调用

```java
package org.xieq.seckill.controller;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;
import org.xieq.system.api.TestFeignClient;

/**
 * @author xieq
 * @date 2023/6/21 14:30
 */
@RestController
public class SeckillController {

    @Autowired
    private TestFeignClient testFeignClient;

    @GetMapping("/seckill")
    public String seckill(){
        return testFeignClient.hello() + "秒杀";
    }
}

```

## seata简介

官方文档: https://seata.io/zh-cn/index.html

## 服务端部署

部署指南:https://seata.io/zh-cn/docs/ops/deploy-guide-beginner.html

下载客户端, 最好根据`springcloudalibaba`版本选择

#### 步骤一：启动包

- [点击下载](https://github.com/seata/seata/releases)

#### 步骤二：建表(仅db)

全局事务会话信息由3块内容构成，全局事务-->分支事务-->全局锁，对应表global_table、branch_table、lock_table

#### 步骤三：修改store.mode

启动包: seata-->conf-->application.yml，修改store.mode="db或者redis"
源码: 根目录-->seata-server-->resources-->application.yml，修改store.mode="db或者redis"

#### 步骤四：修改数据库连接|redis属性配置

启动包: seata-->conf-->application.example.yml中附带额外配置，将其db|redis相关配置复制至application.yml,进行修改store.db或store.redis相关属性。
源码: 根目录-->seata-server-->resources-->application.example.yml中附带额外配置，将其db|redis相关配置复制至application.yml,进行修改store.db或store.redis相关属性。

#### 步骤五：启动

- 源码启动: 执行ServerApplication.java的main方法
- 命令启动: [seata-server.sh](http://seata-server.sh/) -h 127.0.0.1 -p 8091 -m db

```
    -h: 注册到注册中心的ip
    -p: Server rpc 监听端口
    -m: 全局事务会话信息存储模式，file、db、redis，优先读取启动参数 (Seata-Server 1.3及以上版本支持redis)
    -n: Server node，多个Server时，需区分各自节点，用于生成不同区间的transactionId，以免冲突
    -e: 多环境配置参考 http://seata.io/en-us/docs/ops/multi-configuration-isolation.html
```

## 客户端集成

- spring-cloud-starter-alibaba-seata推荐依赖配置方式

```xml
   <dependency>
        <groupId>io.seata</groupId>
        <artifactId>seata-spring-boot-starter</artifactId>
    </dependency>
    <dependency>
        <groupId>com.alibaba.cloud</groupId>
        <artifactId>spring-cloud-starter-alibaba-seata</artifactId>
        <exclusions>
            <exclusion>
                <groupId>io.seata</groupId>
                <artifactId>seata-spring-boot-starter</artifactId>
            </exclusion>
        </exclusions>
    </dependency>
```

- 配置文件

  system

  ```yaml
  seata:
    registry:
      type: nacos
    tx-service-group: system-tx-group
    service:
      vgroup-mapping:
        system-tx-group: default
  ```

  seckill

  ```yaml
  seata:
    registry:
      type: nacos
    tx-service-group: seckill-tx-group
    service:
      vgroup-mapping:
        seckill-tx-group: default
  ```

- 注释配置全局事务注解`@GlobalTransactional`

  ```
      @Override
      @GlobalTransactional
      public Boolean insert() {
  
          Order order = new Order();
          order.setOrderPrice(1000);
          order.setOrderName("电脑订单-" + DateUtil.format(LocalDateTime.now(), "yyyyMMddHHmmss"));
          order.setProductId(1);
  
          productServiceApi.reduce(order.getProductId());
          return this.save(order);
      }
  ```

  

