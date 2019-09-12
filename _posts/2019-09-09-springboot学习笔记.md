> 找各种中文文档，pdf书籍，看官方文档，写代码示例，看系列教程，逐个知识点理解。

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

# 如何实现过滤器
参考 [https://www.cnblogs.com/paddix/p/8365558.html](https://www.cnblogs.com/paddix/p/8365558.html)
### 注册方式实现
- 定义类实现Filter接口，如下
```
package com.example.demo;

import javax.servlet.*;
import javax.servlet.FilterConfig;
import javax.servlet.annotation.WebFilter;
import javax.servlet.http.HttpServletRequest;
import java.io.IOException;

public class LogFilter implements Filter {

    FilterConfig config;

    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        this.config = filterConfig;
    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        HttpServletRequest httpServletRequest = (HttpServletRequest) servletRequest;
        String url = httpServletRequest.getRequestURL().toString();
        long start = System.currentTimeMillis();
        filterChain.doFilter(servletRequest, servletResponse);
        System.out.println("filtername= " + this.config.getFilterName() + " url= " + url + " cost= " + (System.currentTimeMillis() - start));
    }

    @Override
    public void destroy() {

    }
}

```
- 定义config配置类，使用FilterRegistrationBean注册filter
```
package com.example.demo;

import org.springframework.boot.web.servlet.FilterRegistrationBean;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class FilterConfig {
    @Bean
    public FilterRegistrationBean registrationBean() {
        FilterRegistrationBean registrationBean = new FilterRegistrationBean();
        registrationBean.setFilter(new LogFilter());
        registrationBean.addUrlPatterns("/*");
        registrationBean.setName("logFilter");
        registrationBean.setOrder(1);
        return registrationBean;
    }
}
```
### 注解方式实现
- 定义类实现Filter接口，并使用WebFilter注解.WebFilter是Servlet3.0的规范，并不是springboot的注解。

WebFilter的优先级要高于FilterRegistrationBean注册的filter
```
@WebFilter(urlPatterns = "/*", filterName = "logFilter1")
public class LogFilter implements Filter {
}
```
- 在application类中添加ServletComponentScan注解
```
@SpringBootApplication
@ServletComponentScan
public class DemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }

}
```

# 拦截器
- 定义类实现HandlerInterceptor接口
```
@Component
public class LogHandler implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        out.println("------preHandle----------");
        return true;
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, @Nullable ModelAndView modelAndView) throws Exception {
        out.println("------postHandle----------");
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, @Nullable Exception ex) throws Exception {
        out.println("------afterCompletion----------");
    }
}
```
- 定义类注册置，两种方法:

一，实现WebMvcConfigurer接口；
```
@Configuration
public class MvcConfig implements WebMvcConfigurer {

    @Autowired
    private LogHandler logHandler;

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(logHandler).addPathPatterns("/*");
    }
}
```
二，继承类WebMvcConfigurationSupport
```
@Configuration
public class MvcConfig extends WebMvcConfigurationSupport {

    @Autowired
    private LogHandler logHandler;

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(logHandler).addPathPatterns("/*");
        super.addInterceptors(registry);
    }
}
```

# jpa
屏蔽库需要在pom.xml配置文件或这个build.gradle配置文件中将包的引用删掉，并执行clean即可。

