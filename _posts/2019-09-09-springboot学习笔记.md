# 使用pm2守护springboot进程
pm2配置文件如下 pm2.config.js：
```
module.exports = {
    apps: {
		name: "bootgradle-0.0.1-SNAPSHOT.jar",
		script: "java",
		args: [
			"-jar",
			"bootgradle-0.0.1-SNAPSHOT.jar"
		]
	}
};
```
执行pm2 start命令启动脚本：
```
pm2 start pm2.config.js
```

# idea中使用spring-boot-devtools监控源码，实现热启动
spring-boot-devtools默认监控classpath中的改变，当修改完代码后，需要执行
```
Build -> Build Project
```
才能触发重启.
可通过设置：
[Build project automatically](./../assets/img/Build-project-automatically.png)
和按下（shift + option + command + / ）快捷键，Registry，再招到compiler.automake.allow.when.app.running 选项，将它打开。

参考[https://www.cnblogs.com/yjmyzz/p/use-devtools-of-spring-boot-framework.html](https://www.cnblogs.com/yjmyzz/p/use-devtools-of-spring-boot-framework.html)

