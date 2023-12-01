# mysql运维

## window 安装

?> _TODO_

## centos 安装

### yum源安装

?> _TODO_

### 二进制安装

 1. 删除系统自带的`mysql`和`mariadb`

    - 卸载服务

    ```shell
    ## 查询
    rpm -qa | grep mysql
    ## 删除
    rpm -e --nodeps {文件名}

    ## 查询
    rpm -qa | grep mariadb
    ## 删除
    rpm -e --nodeps {文件名}

    ```

    - 删除配置文件

    ```shell
    find / -name mysql|xargs rm -rf
    ```

    - 删除`my.cnf`

    ```shell
    rm -rf /etc/my.cnf
    ```

    - 检查端口

    ```shell
    which mysql
    pkill mysqld
    netstat -lntup|egrep '3306|3307|3308|3309|3316|3326|13306'
    ```

 2. 下载对应的安装包

    [mysql官方下载地址](https://dev.mysql.com/downloads/mysql/)

 3. 解压二进制包

    ```shell
    ## 解压
    tar -xzvf mysql-5.7.43-el7-x86_64.tar.gz
    ```
