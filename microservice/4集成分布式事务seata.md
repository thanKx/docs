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
  
  
