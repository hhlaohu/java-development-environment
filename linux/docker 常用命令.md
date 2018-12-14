#docker 常用命令
https://daocloud.io
login_user_name:hh_laohu@163.com
username:huzhihong
pwd:yf*******

#Docker出现"Cannot connect to the Docker daemon at unix:///var/run/docker.sock. ..."问题
sudo service docker start

# 查看所有镜像
docker images

# 正在运行容器
docker ps

# 查看docker容器
docker ps -a

# 启动tomcat:7镜像
docker run -p 8080:8080 tomcat:7

# 以后台守护进程的方式启动
docker run -d tomcat:7

# 停止一个容器
docker stop b840db1d182b

# 进入一个容器
docker attach d48b21a7e439

# 进入正在运行容器并以命令行交互
docker exec -it e9410ee182bd /bin/sh

# 以交互的方式运行
docker run -i -t -p 8081:8080 tomcat:7 /bin/bash


#删除容器
docker rm container ID

#删除镜像
docker rmi image ID