<p> <a href="../README.md">返回首页</a></p>

## Linux常用命令
- 系统信息查看
```shell
# 操作系统内核信息
uname -a 
# CPU 信息、内核版本
cat /proc/cpuinfo、cat /proc/version
# 查看是否安装了某软件
rpm -qa | grep software
# 后台运行,并且有nohup.out输出
nohub xx &
# 查看最近登录信息列表
last -n 5
# 查看当前目录下各个文件, 文件夹占了多少空间, 不会递归
du -sh *
# 压缩
tar czvf xxx.tar 压缩目录
zip -r xxx.zip 压缩目录
# 解压
tar zxvf xxx.tar
# 解压到指定文件夹
tar zxvf xxx.tar -C /a/b/
unzip xxx.zip
# 复制
cp -r xxx(源文件夹) yyy(目标文件夹)
scp -P ssh端口 username@168.0.0.102:/home/username/xxx /home/xxx #远程复制
# 级联创建目录
mkdir -p /xxx/yyy/zzz
# 比较两个文件
diff -u 1.txt 2.txt
# 检索相关
grep -v xxx xx.log #反向匹配, 查找不包含xxx的内容
# 显示本地打开的所有端口
ss -l 
# 内存使用情况
free -m
# 性能监控
vmstat 2 1 # 2表示每2秒采集一次状态信息, 1表示只采集一次(忽略既是一直采集)
```

## Shell脚本


- 参考：https://www.runoob.com/linux/linux-vim.html

