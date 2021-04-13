### 参考

### 简单版

``` bash

# 1.打包
npm run build

# 2.上传（使用xshell上传dist目录）
rz

# 3.解压缩并且删除没用的压缩文件
unzip dist.zip
rm -rf dist.zip

# 4.部署
docker run -p 80:80 -v $PWD/dist:/usr/share/nginx/html -d nginx

```

[实战Vuepress持续集成](https://www.josephxia.com/ci/#%E5%AE%9E%E6%88%98vuepress%E6%8C%81%E7%BB%AD%E9%9B%86%E6%88%90)