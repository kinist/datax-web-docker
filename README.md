# datax-web-docker

DataX-Web-Docker是面向DataX和DataX-Web进行容器化改造的Project。

其中

DataX项目源地址：https://github.com/alibaba/DataX

DataX-Web项目源地址：https://github.com/WeiYe-Jing/datax-web/

## 安装：

1. 假设需要安装的机器已经完成Docker、Docker-compose的安装，不懂的小伙伴可以自行检索。
2. 将docker-compose.yaml、persistence、build放到服务器同一目录下
3. 启动镜像docker-compose up -d mysql phpmyadmin
4. 在MySQL数据中新建数据库实例：datax_web
5. 执行数据库[初始化脚本](https://github.com/kinist/datax-web-docker/tree/main/sql)，通过访问部署服务器的PMA完成，地址：http://ip:8887/，用户名密码：root/mypassword
6. 停止容器，docker-compose down
7. 编译镜像，docker-compose build
8. 启动容器，docker-compose up -d

## 使用

1. 访问DataX-Web，http://ip:9527/index.html