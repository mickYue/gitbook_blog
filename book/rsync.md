####安装
```
git clone git://git.samba.org/rsync.git
cd rsync
./prepare-source
make && make install
```
####本机使用
`本机使用不需要配置文件，和cp命令类似，一般及时性要求不高的使用crontab定时同步`
- 同步本地两个目录，删除所有不同的文件(--delete表示删除源目录不存在的文件)
rsync -a --delete /home/mick/project/demo/ /data/wwwroot/demo

####跨主机同步
- 配置文件
```
# /etc/rsyncd: configuration file for rsync daemon mode

# See rsyncd.conf man page for more options.

# configuration example:

uid = nobody #rsyncd运行用户
gid = nobody #rsync运行用户组
use chroot =no #一般no,不chroot到path目录
max connections = 4 #最大连接数
pid file = /var/run/rsyncd.pid
ignore errors #可以忽略一些无关的IO错误
secrets file = /etc/rsync.pass #密码和用户名对比表，密码文件自己生成
hosts allow = 10.96.9.105,10.96.9.113#允许主机
hosts deny = 0.0.0.0/0 #禁止主机
# exclude = lost+found/ #排除的文件匹配模式
# transfer logging = yes #是否记录传输日志
timeout = 900 #超时时间秒
ignore nonreadable = yes #是否忽略无权限文件
# dont compress   = *.gz *.tgz *.zip *.z *.Z *.rpm *.deb *.bz2 #传输不压缩的文件格式

[test_module]
path = /data/wwwroot/demo   #rsyncd服务的同步目录
comment = 测试项目同名        
read only = no# 只读
auth users = test #认证用户名，和系统无关
list = yes #客户端请求模块列表是否列出该模块
```
- 密码文件 /etc/rsync.pass,密码文件权限必须600
```
test:123456
```
- 启动rsyncd,默认端口873，netstat -an | grep 873检查是否启动。注意iptables开放端口
```
rsync --daemon --config=/etc/rsyncd.conf
```
- 客户端请求同步文件/home/mick/project/demo/同步到服务器test_module指定的模块
```
  #生成密码文件
  echo "123456">/etc/rsync_client.pass
  chmod 600 /etc/rsync_client.pass
  rsync -avzP --delete --password-file=/etc/rsync_client.pass test@10.20.1.110::test_module /home/mick/project/demo/
```


