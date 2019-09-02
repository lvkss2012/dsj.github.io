# windows下mysql服务停止后启动失败
参考 [data-directory-initialization.html](https://dev.mysql.com/doc/mysql-security-excerpt/5.7/en/data-directory-initialization.html)
> 需要修改my.ini里的datadir和secure-file-priv，于是停止mysql服务，但是重启时报错**本地计算机上的MySQL服务启动后停止。某些服务在未由其他服务或程序使用时将自动停止**。解决方式：
- 修改my.ini文件里的配置地址
```
datadir="g:/mysql/data"
secure-file-priv="g:/mysql/uploads"
```
- 执行命令
```
.\mysqld.exe --defaults-file="C:\ProgramData\MySQL\MySQL Server 8.0\my.ini" --initialize --console
```
会生成临时密码：2019-08-30T06:53:29.996999Z 5 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: Cmu<!015jhi<
- 执行命令移除mysql服务
```
.\mysqld.exe --remove MySQL80
```
- 安装mysql服务
```
.\mysqld.exe --install MySQL80 --defaults-file="C:\ProgramData\MySQL\MySQL Server 8.0\my.ini"
```

