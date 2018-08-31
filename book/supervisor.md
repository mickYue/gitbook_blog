####安装

```
pip install supervisor

```
####生成配置文件
```
echo_supervisord_conf > /etc/supervisord.conf
```
####取消最后include的注释并设置file文件夹路径.

####新建配置文件,示例配置如下
```
[program:webhook] 
command=gitbook serve /website/www.633521.com
;numprocs=1                 ; 默认为1
process_name=webhook
;directory= ; 执行命令前设置workdir
;user=www ;启动进程的用户
;程序崩溃时自动重启，重启次数是有限制的，默认为3次
autorestart=true            
;redirect_stderr=true        ; 重定向输出的日志
stdout_logfile = /var/log/supervisor/webhook.log
loglevel=info　;log等级
```
###启动
```

supervisord -c /etc/supervisord.conf

```
