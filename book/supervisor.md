####安装

```
pip install supervisor

```

####生成配置文件
```
echo_supervisord_conf > /etc/supervisord.conf

```

###启动
```
supervisord -c /etc/supervisord.conf

```
