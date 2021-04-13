
### Dockerfile
Dockerfile 是一个用来构建镜像的文本文件


```bash
# Dockerfile
FROM nginx
RUN echo '这是一个本地构建的nginx镜像' > /usr/share/nginx/html/index.html
```

FROM：定制的镜像都是基于 FROM 的镜像，这里的 nginx 就是定制需要的基础镜像。后续的操作都是基于 nginx。

RUN：用于执行后面跟着的命令行命令。

```bash
docker build -t nginx:test .
# -t 镜像的名字
```

最后的 . 代表本次执行的上下文路径

上下文路径，是指 docker 在构建镜像，有时候想要使用到本机的文件（比如复制），docker build 命令得知这个路径后，会将路径下的所有内容打包。


```bash
FROM node:10-alpine
WORKDIR /usr/src/app
ADD . /usr/src/app
# Npm
RUN npm config set registry https://registry.npm.taobao.org/ && \  
   npm install -g @tarojs/cli && \
   npm i 
# Yarn
# RUN yarn config set registry https://registry.npm.taobao.org && \
#     yarn global add @tarojs/cli && \
#     yarn
#执行构建
CMD ["npm", "run", "build:h5"]
```

::: tip
由于 docker 的运行模式是 C/S。我们本机是 C，docker 引擎是 S。实际的构建过程是在 docker 引擎下完成的，所以这个时候无法用到我们本机的文件。这就需要把我们本机的指定目录下的文件一起打包提供给 docker 引擎使用。

默认上下文路径就是 Dockerfile 所在的位置。

注意：上下文路径下不要放无用的文件，因为会一起打包发送给 docker 引擎，如果文件过多会造成过程缓慢。
:::




### .dockerignore 
.dockerignore 文件的作用类似于 git 工程中的 .gitignore 。不同的是 .dockerignore 应用于 docker 镜像的构建，它存在于 docker 构建上下文的根目录，用来排除不需要上传到 docker 服务端的文件或目录。


docker 在构建镜像时首先从构建上下文找有没有 .dockerignore 文件，如果有的话则在上传上下文到 docker 服务端时忽略掉 .dockerignore 里面的文件列表。

这么做显然带来的好处是：构建镜像时能避免不需要的大文件上传到服务端，从而拖慢构建的速度、网络带宽的消耗；
可以避免构建镜像时将一些敏感文件及其他不需要的文件打包到镜像中，从而提高镜像的安全性；

```bash
# .dockerignore
node_modules
```


### Docker Compose

Compose 是用于定义和运行多容器 Docker 应用程序的工具。通过 Compose，您可以使用 [YML](https://www.runoob.com/w3cnote/yaml-intro.html) 文件来配置应用程序需要的所有服务。然后，使用一个命令，就可以从 YML 文件配置中创建并启动所有服务。


Compose 使用的三个步骤：

使用 Dockerfile 定义应用程序的环境。

使用 docker-compose.yml 定义构成应用程序的服务，这样它们可以在隔离环境中一起运行。

最后，执行 docker-compose up 命令来启动并运行整个应用程序。

```bash
# docker-compose.yml

version: '3.1'
services:
  app-pm2:
      container_name: app-pm2
      #构建容器
      build: ./backend
      #直接从git拉去
      # build: git@github.com:su37josephxia/docker_ci.git#:backend
      # 需要链接本地代码时
      # volumes:
      #   - ./backend:/usr/src/app
      ports:
        - "3000:3000"
  mongo:
    image: mongo
    restart: always
    ports:
      - 27017:27017
  # mongo-express:
  #   image: mongo-express
  #   restart: always 
  #   ports:
  #     - 8081:8081
  nginx:
    restart: always
    image: nginx
    ports:
      - 8091:80
    volumes:
      - ./nginx/conf.d/:/etc/nginx/conf.d
      - ./frontend/dist:/var/www/html/
      - ./static/:/static/

```