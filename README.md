# docker-for-LNMP
a docker development for LNMP
首先这是结合[laradock](https://github.com/LaraDock/laradock)简化后的产物，主要用于形成自己的docker下的开发环境

## 支持的软件 (镜像)

- **数据库引擎**
    - Mysql
    - MongoDB
- **Mysql 管理工具**
    - phpmyadmin
- **缓存引擎:**
    - Redis
    - Memcached
- **搜索引擎**
    - elasticsearch
- **PHP 服务器**
    - Nginx
- **PHP 进程管理**
    - php-worker
- **PHP 编译工具**
    - php-fpm (php7.2)
- **工具:**
    - Workspace (PHP7-CLI, SOAP, xDebug, Composer, Git, Node, YARN, Gulp, SQLite, Vim, Nano, cURL...)

## 操作

1. 克隆此仓库到你系统的任意位置（建议在你的工作目录）：

```bash
git clone https://github.com/zy943453722/docker-for-LNMP
cd DevDock
cp .env.example .env
```
其中的.env是已经配好的适应于win环境的一套变量配置

2. 根据自己的需求修改每个软件的dockfile并保存，dockfile的具体语法请见  (https://docs.docker.com/engine/reference/builder/)

3. 根据自己的需求修改docker-compose.yml文件并保存，docker-compose具体语法请见
(https://docs.docker.com/compose/compose-file/)

4. 而后运行
```
docker-compose up -d mysql nginx redis php-worker  ...........(任何支持的你想配置的软件)
因为php-fpm和workspace是很多软件depend on的，因此不需要刻意创建，会自动创建。
```

## 使用


### 提升 Mac 系统上的项目访问速度

在 Mac 系统上，Docker 运行在一个特别的虚拟机上，当容器访问挂载的数据卷中的文件时会出现极其缓慢的现象，这会浪费了我们很多时间，现在解决方案来了！我们开始使用 Docker Sync ，只需要先执行 `./sync.sh install`，然后将常用命令 `docker-compose up -d`  替换成 `./sync.sh up` ，`docker-compose down` 替换成 `./sync.sh down` 即可，想要了解更多关于 Docker Sync 的细节，请访问 ![Docker Sync](https://github.com/EugenMayer/docker-sync)

### 灵活配置开发环境

在 docker-compose.yml 中，引用了很多环境变量，可自行在 .env 进行配置。典型的，我已经将 nginx 目录下 的 sites 目录映射到 nginx 容器，所以当你修改 nginx 网站配置文件后，只要重启 nginx 容器即可：

`docker-compose restart nginx`

### 常用命令

* 列出正在运行的所有容器

`docker ps`

* 关闭所有容器

`docker-compose stop`

* 停止某个容器:

`docker-compose stop {container_name}`

* 删除服务容器

`docker-compose down {container_name}`

该命令不会删除你的数据卷容器，如果你重新创建服务容器，服务容器默认仍会使用上次创建的数据卷容器,如果不加 {容器名称} ，命令会删除所有服务容器。

* 列出所有数据卷容器

 `docker volume ls` 

* 删除数据卷容器

 `docker volume rm <VOLUME NAME>`

* 删除所有数据卷容器

 `docker volume rm $(docker volume ls -q)`

* 删除所有未被使用的数据卷容器

 `docker volume rm $(docker volume ls -qf dangling=true)`

* 查看容器日志

Nginx 的日志在 logs/nginx 目录
查看其它容器日志 (Mysql, php-fpm, …) 你可以运行:

 `docker-compose logs {image-name}`

* 如果你做任何改变 Dockerfile 确保你运行这个命令,可以让所有修改更改生效:

`docker-compose build`

* 选择你可以指定哪个镜像 (而不是重建所有的镜像):

`docker-compose build {image-name}`


* 如果你想重新创建整个镜像，你需要使用 --no-cache 选项  

`docker-compose build --no-cache {container-name}`

* 进入容器

`docker exec -it {container-name} bash`

**注：以上命令请在win10下的powershell下执行，默认是tty**

### 特别注意：xdebug+phpstorm的配置


