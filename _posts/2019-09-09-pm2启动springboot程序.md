- 使用pm2守护springboot进程
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

