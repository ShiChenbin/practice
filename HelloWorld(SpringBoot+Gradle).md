# 任务
基于 java、Spring Boot、gradle搭建一个hello world 项目

# 在搭建过程中的疑问
## Gradle 和 Maven 的区别
Gradle和Maven都是项目自动构建工具，通过自定义构建逻辑，然后交给Gradle 或者 是Maven去完成。其中二者的区别在于，Gradle不需要Maven繁琐的XML配置，即Gradle与Maven相比更加的灵活。

但同时Gradle也有不可忽视的缺点，Gradle在大项目中编译很慢而且容易内存溢出。
（Spring Boot 2.3.1开始已经切换到Gradle）
## 开发环境、测试环境、生产环境 到底是什么？
[答](https://blog.csdn.net/qq_36538012/article/details/78552074)

# 具体搭建
1.进入[Spring网站](https://start.spring.io/)生成一个初始化的压缩包。
![请添加图片描述](https://img-blog.csdnimg.cn/117024b6258243888357a24cfd0524c1.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5pyI5YWJ5LiN5p-T5piv6Z2e,size_20,color_FFFFFF,t_70,g_se,x_16)
点击GENERATE然后下载后解压。
2.打开idea，File->Open->找到解压文件存放的地方，并找到build.gradle。
![请添加图片描述](https://img-blog.csdnimg.cn/02af01dde64c4e0899316c6b53d79029.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5pyI5YWJ5LiN5p-T5piv6Z2e,size_20,color_FFFFFF,t_70,g_se,x_16)
点击然后Open as Project。
![在这里插入图片描述](https://img-blog.csdnimg.cn/bb796c71cd4a4887a689d92da6f92dba.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5pyI5YWJ5LiN5p-T5piv6Z2e,size_20,color_FFFFFF,t_70,g_se,x_16)
对Project下的build.gradle进行修改
我的配置信息如下，

 - 关于repositories 当中源的问题，我更改成国内源会出现信任问题，然而加上allowInsecureProtocol也没有发生改变，所以直接默认来看也是没什么问题的。

```groovy
//插件
plugins {
	id 'org.springframework.boot' version '2.6.3'
	id 'io.spring.dependency-management' version '1.0.11.RELEASE'
	id 'java'
}

group = 'com.example'
version = '0.0.1-SNAPSHOT'
sourceCompatibility = '17'

repositories {
	mavenCentral()
	//将上方语句注释掉，并更改成Gradle的国内源,并且需要信任
	//allowInsecureProtocol = true
	//url "http://maven.aliyun.com/nexus/content/groups/public/"
}

dependencies {
	implementation 'org.springframework.boot:spring-boot-starter-web'
	testImplementation 'org.springframework.boot:spring-boot-starter-test'
	//classpath("org.springframework.boot:spring-boot-gradle-plugin:${springBootVersion}")//添加
}

tasks.named('test') {
	useJUnitPlatform()
}

```

在该目录下新建一个controller包，书写页面控制器。
![在这里插入图片描述](https://img-blog.csdnimg.cn/8c455fdd65764c009d96d50adc98e33c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5pyI5YWJ5LiN5p-T5piv6Z2e,size_20,color_FFFFFF,t_70,g_se,x_16)
新建控制器类HelloController
![在这里插入图片描述](https://img-blog.csdnimg.cn/abe3e11413be49ee82e90ffa76107720.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5pyI5YWJ5LiN5p-T5piv6Z2e,size_20,color_FFFFFF,t_70,g_se,x_16)
HelloController中的内容如下，
```java
package com.example.demo.controller;

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloController {
    @RequestMapping(value = "hello")
    public String hello(){
        return "Hello world.Hello Spring boot and gradle";
    }
}

```
DemoApplication是我们这个项目的启动类，所以需要放置与controller包同一级下，直接拖动即可，最后结果如图。
![在这里插入图片描述](https://img-blog.csdnimg.cn/c6772bc4bc1541209ceaaff44286523c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5pyI5YWJ5LiN5p-T5piv6Z2e,size_20,color_FFFFFF,t_70,g_se,x_16)
run。
![在这里插入图片描述](https://img-blog.csdnimg.cn/aafe743651984dd286e2547d59c2dddc.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5pyI5YWJ5LiN5p-T5piv6Z2e,size_20,color_FFFFFF,t_70,g_se,x_16)
打开网页[http://localhost:8080/hello](http://localhost:8080/hello)，如果上述代码的value不同自行对其进行修改。
