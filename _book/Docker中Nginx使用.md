拉取官方镜像 - 面向docker的只读模板 

```
docker pull nginx 
```
 
查看 
```
docker images nginx
```
 
启动镜像 
```
mkdir www echo 'hello docker!!' >> www/index.html 
```

启动 # www目录里面放一个index.html 
```
//$PWD   当前目录
docker run -p 8000:80 -v $PWD/www:/usr/share/nginx/html nginx
```
 
后台启动 
```
docker run -p 80:80 -v $PWD/www:/usr/share/nginx/html -d nginx
```

停止
```
docker stop ff6
```
 
查看进程 
```
docker ps docker ps -a // 查看全部
```
 
伪终端 ff6容器的uuid 
```
docker exec -it ff6 /bin/bash
```
 
删除镜像 
```
docker rm ff6
```