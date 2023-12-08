# nginx运维

## 参考地址

[nginx-turtorial](https://dunwu.github.io/nginx-tutorial/#/README): github上整理的nginx运维知识

## Linux安装

### rpm包安装

### 编译安装

1、下载解压到本地

[官方地址](http://nginx.org/en/download.html): 选择合适的版本下载。

```bash
cd /opt/nginx
tar -xzvf nginx-1.xx.xx.tar.gz
```

2、编译安装

执行一下命令：

```bash
cd /opt/nginx-1.xx.xx
./configure --with-http_stub_status_module --with-http_ssl_module --with-pcre=/opt/pcre/pcre-8.35
make && make install
```

3、关闭防火墙

```bash
firewall-cmd --zone=public --add-port=80/tcp --permanent
firewall-cmd --list-ports
firewall-cmd --reload
```

4、开机自启动nginx

创建项目文件

```bash
vim /lib/systemd/system/nginx.service
```

填写下面内容填入（注意nginx安装路径）

```bash
[Unit]
Description=nginx
After=network.target

[Service]
Type=forking
ExecStart=/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
ExecReload=/usr/local/nginx/sbin/nginx -s reload
ExecStop=/usr/local/nginx/sbin/nginx -s quit
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```

5 问题

SSL配置测试不同，很有可能是因为`--with-http_ssl_module`参数没有配置
