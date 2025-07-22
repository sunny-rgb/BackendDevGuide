# docker安装配置与使用

## 1. Docker 简介



## 2. Docker 安装与配置

- docker学习资料总结

  [docker desktop 官方网站](https://docker.p2hp.com/get-started/index.html)

  [docker 官方镜像仓库空间](https://hub.docker.com/)

  [docker desktop 安装](https://blog.csdn.net/weixin_43152440/article/details/146691801)

  [docker 安装与教学（40分钟，讲解非常清晰，强烈建议完整看完！）](https://www.bilibili.com/video/BV1THKyzBER6/?spm_id_from=333.337.search-card.all.click&vd_source=bf13787311127d9efdb95deea8b81a48)

- docker windows 安装

  1. 以管理员身份运行 cmd ，执行下列指令，安装新版本的 `wsl` 。
  
  ```bash
  wsl --set-default-version 2
  wsl --update --web-download
  ```
  
  2. 在 [docker desktop 官方网站](https://docker.p2hp.com/get-started/index.html) 下载 windows 版本的 docker desktop 安装包，并安装到指定的 D盘。
  
  ```bash
  start /w "" "Docker Desktop Installer.exe" install --backend=wsl-2 --installation-dir=D:\Program Files\Docker\ --accept-license
  ```
  
  3. 启动 `docker desktop 桌面应用程序` ，并 [配置镜像站](https://blog.csdn.net/CSDN1027719307/article/details/149422544) 。

## 3. Docker 常用命令

1. 下载镜像，从 Docker官方仓库 里面下载最新版本的 mysql 镜像

   >docker pull nginx
   
2. 查看已经下载过的全部镜像

   >docker images

3. 删除镜像

   >docker rmi <IMAGE ID>

4. 删除容器

   >docker rm -f <CONTAINER ID>

5. 创建并运行 docker容器。需要注意的是， `docker pull` 可以省略，当发现本地不存在这个镜像，会自动拉取。此外，不同的可选参数代表不同的含义。

   - **-d**：让容器在后台运行（detached mode），不会占用我们当前的终端
   - **-p  8080:80**：由于宿主机的网络和容器内的网络并不相连，要正常访问容器内启动的诸如 `mysql` 、`nginx` 服务，就必须进行宿主机到容器内服务端口号的一个映射，把宿主机的 8080 端口映射到容器的 80 端口（nginx 默认端口）。
   - **-v D:\data:/app/data** 。把本地 `D:\data` 文件夹挂载到容器中的 `/app/data` 目录。这个和 `-p` 的作用类似，是把宿主机和容器的文件目录进行绑定。宿主机目录和容器内目录的修改，分别会互相影响，宿主机与容器机通过这个目录紧密的联系到了一起，实现宿主机和容器之间共享数据（读写同步），这种目录也被称为 **挂载卷**。挂载卷的最大作用是数据的持久化保存。当我们删除容器的时候，容器内的数据会被全部删除掉。如果我们使用了挂载卷，那么容器内目录的数据就会保存在宿主机的对应目录里面。这样删除了容器的时候，数据不会被删除。
   - **--name**：自定义容器的名字
   - **-e**：传递环境变量，常用于向容器传递配置信息，比如数据库用户名、密码、API 密钥等。
   - **--restart always**： 容器自动重启策略选项，它用于控制容器在异常退出或 Docker 重启后是否自动重启。
   - **--restart unless-stopped**：因为意外情况停止的容器会重启，但是手动停止的容器不会自动重启。

   >docker run -d -p 8080:80 -v D:\data:/app/data nginx

6. 查看容器状况

   - **-a**：包括正在运行的和已经停止的

   >docker ps -a

7. 停止容器运行

   >docker stop <CONTAINER NAME>

8. 启动容器运行

   >docker start <CONTAINER NAME>

9. 创建 docker容器，和 `docker run` 命令非常相似，但是区别是 `docker create` 只创建容器，但是不立即启动。

   >docker create

10. 创建挂载卷

    >docker volume create <VOLUME NAME>

11. 使用挂载卷 创建并运行docker容器

    >docker run -d -p 8080:80 -v <VOLUME NAME>:/app/data nginx

12. 列出所有 VOLUME 卷

    >docker volume list

13. 删除 VOLUME 挂载卷

    >docker volume rm <VOLUME NAME>

## 4. Dockerfile

什么是 `Dockerfile` ？如何使用 `Dockerfile` 创建镜像？如何把镜像推送到 `Docker Hub` 上面？

## 5. Docker 网络

## 6. Docker compose

`docker compose` 是 Docker 提供的一种工具，用于**定义和管理多个容器服务**，可以通过一个 YAML 文件一次性启动整个应用栈（如 Web 服务 + 数据库 + Redis）。本质上是 `docker compose` 让你用一个配置文件，通常是 `docker-compose.yml` ，来管理多个容器服务，避免写一堆 `docker run` 命令。