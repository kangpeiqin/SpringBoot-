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
- 安装 `MongoDB`
```bash
docker run --name mongo \
-p 27017:27017 --restart=always \
-v /data/mongo/data:/data/db \
-v /data/mongo/conf:/data/conf \
-v /data/mongo/log:/data/log \
-d mongo

docker exec -it mongo mongo admin
db.createUser({user:'admin',pwd:'admin',roles:[{role:'readWrite',db:'admin'}],})
db.auth('admin','admin')
show users
db.createUser({user:'admin',pwd:'admin',roles:[{role:'readWrite',db:'mall-portal-dev'}]})
db.updateUser('admin', {roles:[{role:"readWrite",db:"mall-portal-dev"}]});
use database
db.createUser({user:'admin',pwd:'admin',roles:[{role:'readWrite',db:'mall-portal-dev'}]})
```
> `MongoDB` 配置文件(`mongodb.conf`)
```properties
#端口
port=27017
#数据库文件存放目录
dbpath=/data/mongo/data
#日志文件存放路径
logpath=/data/mongo/log
#使用追加方式写日志
logappend=true
#以守护线程的方式运行，创建服务器进程
fork=true
#最大同时连接数
maxConns=100
#不启用验证
#noauth=true
#每次写入会记录一条操作日志
journal=true
#存储引擎有mmapv1、wiredTiger、mongorocks
storageEngine=wiredTiger
#访问IP
bind_ip=0.0.0.0
#用户验证
#auth=true
```
- `ES` 和 `kibana` 安装
```bash
# 创建目录 /data/es 及其子目录，设置权限
chmod 777 /data/es
# 下载镜像
docker pull docker.elastic.co/elasticsearch/elasticsearch:7.6.2
docker pull docker.elastic.co/kibana/kibana:7.6.2
docker pull docker.elastic.co/logstash/logstash:7.6.2 
# 查看镜像ID
docker image 
# 安装 ES
docker run --name elasticsearch -p 9200:9200 -p 9300:9300 \
--restart=always \
-e "discovery.type=single-node" \
-e ES_JAVA_OPTS="-Xms512m -Xmx1024m" \
-v /data/es/config/elasticsearch.yml:/usr/share/elasticsearch/config/elasticsearch.yml \
-v /data/es/data:/usr/share/elasticsearch/data \
-v /data/es/plugins:/usr/share/elasticsearch/plugins \
-d container_id
# ES 配置文件修改
echo "http.host: 0.0.0.0">> /data/es/config/elasticsearch.yml
# 安装 kibana
docker run --name kibana -e ELASTICSEARCH_HOSTS=http://192.168.150.58:9200 -p 5601:5601 -d container_id
# 下载 IK 分词器
https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v7.6.2/elasticsearch-analysis-ik-7.6.2.zip
# 测试是否安装成功
POST _analyze
{
  "analyzer": "ik_max_word",
  "text":"测试数据"
}
# 将压缩包放到 /data/es/plugins/ik 目录下,重启ES
docker restart elasticsearch
# logstash
docker run --name logstash -p 4560:4560 -p 4561:4561 -p 4562:4562 -p 4563:4563 \
--restart=always \
-e ES_JAVA_OPTS="-Xms512m -Xmx1024m" \
-v /data/logstash/config:/usr/share/logstash/config \
-v /data/logstash/pipeline:/usr/share/logstash/pipeline \
-d fa5b3b1e9757

docker run -d --name=logstash fa5b3b1e9757

docker cp logstash:/usr/share/logstash/config /data/logstash/
docker cp logstash:/usr/share/logstash/pipeline /data/logstash/

# 进入logstash容器
docker exec -it logstash /bin/bash
# 进入bin目录
cd /bin/
# 安装插件
logstash-plugin install logstash-codec-json_lines
# 退出容器
exit
# 重启logstash服务
docker restart logstash
```
- `RabbitMQ` 安装
```bash
mkdir -p /data/rabbitmq/{data,conf,log}
chmod -R 777 /data/rabbitmq #   授权

docker run -p 5672:5672 -p 15672:15672 \
-v /data/rabbitmq/data:/var/lib/rabbitmq \
-v /data/rabbitmq/conf:/etc/rabbitmq \
-v /data/rabbitmq/log:/var/log/rabbitmq \
--name rabbitmq -d rabbitmq:management

docker exec -it rabbitmq bash
rabbitmq-plugins enable rabbitmq_management

docker exec -it rabbitmq /bin/bash
rabbitmqctl add_user admin admin #创建用户 
rabbitmqctl set_user_tags admin administrator #给用户授权角色
rabbitmqctl set_permissions -p / admin "." "." ".*" #给用户添加权限
```
