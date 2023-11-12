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



