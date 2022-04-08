<p> <a href="../README.md">返回首页</a></p>

# Linux常用命令
## 重定向
**I/O 重定向可以改变输出内容发送的目的地，也可以改变输入内容的来源地。**
默认情况下，输出内容显示在屏幕上，输入内容来自键盘。
- 标准输出重定向
> 把标准输出(默认是屏幕)重定向到另外一个文件中
```bash
# 把标准输出重定向到另外一个文件中
ls -l /usr/bin > output.txt
# 删除文件内容(创建一个空内容的文件)
> output.txt
# 重定向(追加)
ls -l /usr/bin >> output.txt
```
- 标准错误重定向
> 将命令执行错误时输出的内容进行重定向
```bash
# 将标准错误重定向到某个文件，标准错误等同于文件描述符2
ls -l /bin/usr 2> error.txt
# 将标准输出和错误重定向到同一个文件当中
ls -l /bin/usr &> ls-output.txt
# 隐藏一个命令的错误信息，把输出重定向到一个称为/dev/null的特殊文件(接收输入，但不对输入内容进行任何处理)  
ls -l /bin/usr 2> /dev/null
```
- 标准输入重定向
> 指定输入的来源(默认为键盘)
```bash
# cat：读取一个或多个文件，并把它们复制到标准输出文件中
cat [file ...]
# 将标准输入内容复制到指定文件中
cat > log.txt
# 改变标准输入来源(来自指定文件)
cat < log.txt
```
- 管道
> 管道操作符号"|"可以把一个命令的标准输出作为一个命令的标准输入
```bash
# 将输出结果进行分页显示
ls -l /usr/bin | less
# 将数据进行过滤处理后进行显示，排序去重后进行分页显示
ls /bin /usr/bin | sort | uniq | less
# wc: 打印行数、字数、字节数
wc output.txt
# grep: 在文件中查找匹配文本，忽略大小写(-i)
grep pattern [file...]
# 获取系统中与文件压缩相关的程序
ls /bin /usr/bin | sort | uniq | grep zip
# head/tail：打印文件的开头或者结尾部分
ls /usr/bin | tail -n 5
# 实时查看文件(-f)，观察正在被写入的日志文件的进展状态，可以用于查看实时日志情况
tail -n 200 -f log.txt
# 捕获一个管道当中的内容：tee
ls /usr/bin | tee ls.txt | grep zip
```
## 文件的搜索
```bash
# locate：通过快速搜索数据库，更新数据库：updatedb
locate zip | grep bin
# locate 查找文件仅仅是依据文件名，而 find 程序则是依据文件的各种属性在既定的目录（及其子目录）里查找 
# ~ 中目录总量
find ~ -type d | wc -l
```
## 归档和备份
压缩：分为有损(JPEG、MP3)/无损压缩。**归档是一个聚集众多文件并将它们组合为一个大文件的过程。**
```bash
# gzip 命令用于压缩一个或者多个文件，执行命令后，源文件会被压缩文件取代
gzip/gunzip output.txt
# tar(tape archive) 归档文件可以由许多独立的文件、一个或多个目录层次或者两者的混合组合而成
tar -czvf foo.tar.gz foo.txt # 打包并压缩
tar -xzvf foo.tar.gz # 解压
# zip 打包压缩工具
zip foo.zip foo.txt
unzip foo.zip
```
## 网络
- 网络检测、监测
```bash
# 跟踪网络数据包的传输路径，显示数据连接所经过的站点(netstat)
traceroute www.baidu.com
# 检查网络设置及其相关统计
netstat -ie
```
- 文件传输与下载
```bash
# 用于文件下载的命令行程序：wget，可以用于从网站上下载内容也可以用于从FTP站点下载，单个文件、多个文件甚至整个网站都可以被下载。
wget www.baidu.com
# scp(secure copy)：从远程主机复制文件
scp remote-sys:document.txt .
```
## 软件包管理
在系统上安装、维护软件的方法。主要的两种方式：`Debian`的`.deb`技术和`Red Hat`的`.rpm`技术。
软件包管理系统通常包含两类工具——执行如安装、删除软件包文件等任务的低级工具(`dpkg`、`rpm`)和进行元数据搜索及提供依赖性解决的高级工具(`apt-get`、`yum`)。
```bash
# 安装、更新、卸载
yum install/update/erase
# 列出已经安装的软件包列表
rpm -qa
```
## 进程
通过快速切换运行中的程序来实现多任务的同时执行。
```bash
# 进程信息查看
ps -aux
# 动态查看进程信息
top
```
## 存储介质
- 挂载、卸载存储设备
> 管理存储设备首先要做的就是将该设备添加到文件系统树中，从而允许操作系统可以操作该设备，这个过程称之为挂载。
## shell 脚本
## 环境（environment）

## 参考：
- https://www.runoob.com/linux/linux-vim.html
- 《Linux 命令行大全》
