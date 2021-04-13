### 拉取mysql镜像

```bash
docker pull mysql:5.7
```

### 运行 mysql
```bash
docker run -itd  -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 mysql

# 参数说明

# -p 3306:3306 ：映射容器服务的 3306 端口到宿主机的 3306 端口，外部主机可以直接通过 宿主机ip:3306 访问到 MySQL 的服务。
# MYSQL_ROOT_PASSWORD=123456：设置 MySQL 服务 root 用户的密码。

```


### 进入mysql终端

```bash
docker exec -it e7c bash 
# e7c是我启动的mysql服务的CONTAINER ID

mysql -u root -p
# 进入数据库

exec
# 退出
```


::: warning
在navicat链接mysql8以后的版本时，会出现2059的错误，这个错误出现的原因是在mysql8之前的版本中加密规则为mysql_native_password，而在mysql8以后的加密规则为caching_sha2_password。解决此问题有两种方法，一种是更新navicat驱动来解决此问题，一种是将mysql用户登录的加密规则修改为mysql_native_password。

本文采用第二种方式。

ALTER USER 'root'@'39.102.60.136' IDENTIFIED BY '123456' PASSWORD EXPIRE NEVER

ALTER USER 'root'@'39.102.60.136' IDENTIFIED WITH mysql_native_password BY '123456'
:::


[mysql8.0报2059解决方案](https://jingyan.baidu.com/article/0aa22375e7966ac8cc0d64b3.html)



