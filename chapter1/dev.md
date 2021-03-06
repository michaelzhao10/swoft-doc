# 开发指南

## 如何在 Windows 下开发

### Windows 10
1. Win10 可使用 [Docker for Windows](https://www.docker.com/docker-windows) 搭建基于 Docker 的环境，具体 Docker 环境可参考下一章节
2. 通过 [Vagrant](https://www.vagrantup.com) 或其它虚拟机系统搭建 Linux 系统环境

### 低于 Windows 10
1. 可使用 [Docker toolbox](https://docs.docker.com/toolbox/) 搭建基于 Docker 的环境，具体 Docker 环境可参考下一章节
2. 通过 [Vagrant](https://www.vagrantup.com) 或其它虚拟机系统搭建 Linux 系统环境

## 基于 Docker 构建一个便于开发调试的环境
建议通过 [docker-compose](https://docs.docker.com/compose/) 来启动相关环境，下面是一个参考的`yaml`

```yaml
version: '2'
services:
    swoft-dev:
        image: swoft/swoft:latest
        ports:
            - "80:80"
        volumes:
            - /usr/swoft:/var/www
        stdin_open: true
        tty: true
        command: /bin/bash
```
其中`volumes`下的`/usr/swoft`需更换成本地实际Swoft项目的地址，`"80:80"`的第一个80替换成需要绑定的宿主机端口

下面给出一个实际使用的参考流程

* 安装 Docker 环境及 docker-compose ，并启动 Docker 服务

* 编写 `docker-compose.yml` 文件

* 在当前目录执行 `docker-compose up -d`

* 启动成功后执行 `docker ps` 查看容器信息，下为示例
```
CONTAINER ID  IMAGE               COMMAND                 CREATED             STATUS             PORTS               NAMES
f22173763374  swoft/swoft:latest  "docker-php-entrypoin"  About a minute ago  Up About a minute  0.0.0.0:80->80/tcp  env_swoft-dev_1
```
* 得知容器ID为 `f22173763374` ，容器名称为 `env_swoft-dev_1`

* 执行 `docker exec -it f22173763374 bash` 或 `docker exec -it env_swoft-dev_1 bash` 进入容器

* 由于 command 已经更换为 bash, 所以其实此时 Swoft 并未启动，`cd /var/www/swoft` CD 到项目目录，执行 `php bin/swoft start` 启动 Swoft，需要重启 Swoft 直接 `CRTL + C` 终止并重新启动即可

## PHPStorm 安装 `PHP Annotations` 插件优化注解使用
通过文本编辑器的环境进行 Swoft 开发，在使用 Swoft 便捷的注解时，仍需要 use 注解相对应的命名空间， 这显然不是一个高效的做法。

我们推荐在 `PHPStorm` 环境下，并安装 Jetbrain 自带的 `PHP Annotations` 插件，可提供注解命名空间自动补全，注解属性代码提醒，注解类跳转等非常有助于提升开发效率的功能。



