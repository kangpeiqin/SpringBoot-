## `CI/CD` 持续集成和持续交付
记录一下如何使用 `Jenkins` 进行项目自动化部署
### `Jenkins` 自动化部署的原理
> Jenkins 可以使用服务器提供的登录凭证，登录到远程服务器当中，执行 `Shell` 脚本从而实现自动化部署。此外，Jenkins 也提供了
很多插件进行功能扩展，如 Git、Docker、HTTP 等插件，这样可以使用这些工具提供的一些命令，如拉取代码、Docker 部署等。 

![原理](/asset/images/jenkins.jpg "Magic Gardens")

> 另外可以使用 Docker 进行部署，这样的优势是：一次构建，到处运行。可以通过网络，快速分发运行打包好的软件：
我们可以从镜像仓库快速获取构建好的镜像，然后生成容器直接运行，就像直接从应用市场里直接获取软件进行安装一样方便，
可以帮我们省去服务迁移重新打包构建部署等繁琐的步骤。
### 基础环境配置与搭建
#### Docker 的使用与安装
具体可以参考：https://vuepress.mirror.docker-practice.com/，以下是简单的安装流程
```bash
# 使用脚本自动安装
curl -fsSL get.docker.com -o get-docker.sh
sudo sh get-docker.sh --mirror Aliyun
# 启动 Docker 并设置开机启动
sudo systemctl enable docker
sudo systemctl start docker
```
#### 使用 Docker 安装基础软件
- 安装 `Jenkins`
```bash
docker run -d -p 8060:8080 -p 50000:50000 \
--user=root \
--name jenkins --privileged=true \
--restart=always \
-v /data/jenkins/jenkins_home:/var/jenkins_home \  #Jenkins 目录挂载
-v /data/maven/apache-maven-3.6.3:/usr/local/maven \
-v /data/jdk/jdk1.8.0_161:/usr/local/java \
-v /var/run/docker.sock:/var/run/docker.sock \  #Docker 相关目录挂载，这样做才能使用 Docker 命令
-v /usr/bin/docker:/usr/bin/docker \    
-v /data/node/node-v14.16.1-linux-x64:/usr/local/nodejs \ #NodeJS 目录挂载
jenkins/jenkins
```
- 安装 `NodeJS`
> 用于前端工程打包
```bash
cd /data/nodejs
wget https://npm.taobao.org/mirrors/node/v14.16.1/node-v14.16.1-linux-x64.tar.xz
xz -d node-v14.16.0-linux-x64.tar.xz
tar -xzvf node-v14.16.0-linux-x64.tar
# 配置环境变量
export NODE_HOME=/data/nodejs/node-v14.16.1-linux-x64
export PATH=$NODE_HOME/bin:$PATH
# 检测是否安装成功
node -v
```
- 安装 `Maven`
```bash
cd /data/maven
wget https://mirrors.aliyun.com/apache/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.tar.gz
# 解压缩
tar -xzvf apache-maven-3.6.3-bin.tar.gz
# 环境变量配置：添加环境变量
vim /etc/profile
export M2_HOME=/data/maven/apache-maven-3.6.3                                         export PATH=$PATH:${M2_HOME}/bin
# 刷新配置
source /etc/profile         
```
- 安装 `Nginx`
```bash
# 从容器当中复制配置文件到指定目录
docker run -p 80:80 --name nginx
docker cp nginx:/etc/nginx/nginx.conf /data/nginx
docker cp nginx:/etc/nginx/conf.d/ /data/nginx
docker stop nginx
docker rm nginx
# 生成容器并挂载目录
docker run -p 80:80 --name nginx --restart always \
--net host \
-v /data/nginx/html:/usr/share/nginx/html \  
-v /data/nginx/nginx.conf:/etc/nginx/nginx.conf \ #主配置文件挂载
-v /data/nginx/conf.d:/etc/nginx/conf.d \ 
-v /data/nginx/logs:/var/log/nginx -d nginx  # 日志挂载
```