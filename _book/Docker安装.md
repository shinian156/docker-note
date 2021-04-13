### 安装环境  centos:7.7

<!-- -v $PWD/www:/www把主机当前目录下的www目录绑定到了docker中www目录。需要注意的是， -->



1. Docker 要求 CentOS 系统的内核版本高于 3.10 ，查看本页面的前提条件来验证你的CentOS 版本是否支持 Docker 。

通过 uname -r 命令查看你当前的内核版本
```bash
 $ uname -r
 ```
2. 使用 root 权限登录 Centos。确保 yum 包更新到最新。
```bash
$ sudo yum update
```
3. 卸载旧版本(如果安装过旧版本的话)
```bash
$ sudo yum remove docker  docker-common docker-selinux docker-engine
```
4. 安装需要的软件包， yum-util 提供yum-config-manager功能，另外两个是devicemapper驱动依赖的
```bash
$ sudo yum install -y yum-utils device-mapper-persistent-data lvm2
```
5. 设置yum源
```bash
$ sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

6. 可以查看所有仓库中所有docker版本，并选择特定版本安装
```bash
$ yum list docker-ce --showduplicates | sort -r
```

7. 安装docker
```bash
sudo yum install docker-ce docker-ce-cli containerd.io
``` 

8. 启动并加入开机启动
```bash
$ sudo systemctl start docker
```

9、验证安装是否成功(有client和service两部分表示docker安装启动都成功了)
```bash
$ docker version
```



::: danger 错误总结
Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?

重新加载某个服务的配置文件，如果新安装了一个服务，归属于 systemctl 管理，要是新服务的服务程序配置文件生效，需重新加载。


<!-- systemctl daemon-reload
sudo service docker restart
sudo service docker status  -->


systemctl docker restart   重启docker

docker run images          重新运行

:::
