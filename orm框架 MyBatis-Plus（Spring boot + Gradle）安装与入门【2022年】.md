# 前言
写此文的原因在于，由于软件版本迭代的太快了，网上的许多教程已经不合适了，同时有许多命令并不太懂其含义。
如果觉得本篇章对您有所帮助，希望能给我一个赞，稍稍鼓励一下，非常感谢。


# 官方教程
[传送门](https://baomidou.com/pages/bab2db/#release)

# 我的版本信息
- java 17.0.1（后面修改成8了，如果有差异会在文章中注明）
- gradle 7.4
- mysql 8.0
- mybatis plusv 3.5.1 [官方版本更新日志](https://github.com/baomidou/mybatis-plus/blob/3.0/CHANGELOG.md)

# 安装教程
1. 首先创建一个Spring boot（Gradle）项目，具体教程与此入门教程[传送门](https://blog.csdn.net/qq_43382350/article/details/122976481?spm=1001.2014.3001.5502)一致

2. 安装相关依赖

Spring Boot
Maven：
```java
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.5.1</version>
</dependency>
```
Gradle:
- 打开build.gradle
```java
compile group: 'com.baomidou', name: 'mybatis-plus-boot-starter', version: '3.5.1'
```
- 由于新版取消了compile的用法，所以，无法运行或者是出现报错的可以更改为如下
```java
implementation group: 'com.baomidou', name: 'mybatis-plus-boot-starter', version: '3.5.1'
```
版本号version可以根据官方的进行具体的修改

其他配置
```java
dependencies {
	//SpringBoot 基础配置
	implementation 'org.springframework.boot:spring-boot-starter-web'
	testImplementation 'org.springframework.boot:spring-boot-starter-test'

	//Mybatis-plus(简称MP)依赖
	implementation group: 'com.baomidou', name: 'mybatis-plus-boot-starter', version: '3.5.1'

	//lombok java库依赖
	compileOnly 'org.projectlombok:lombok'
	annotationProcessor 'org.projectlombok:lombok'

	//devtool 用于配置热部署的，可以删去
	developmentOnly 'org.springframework.boot:spring-boot-devtools:2.6.3'

	//mysql数据库依赖
	runtimeOnly 'mysql:mysql-connector-java'

	//其他
	testImplementation('org.springframework.boot:spring-boot-starter-test') {
		exclude group: 'org.junit.vintage', module: 'junit-vintage-engine'
	}

}

```

- 然后点击右侧的Gradle进行刷新，如图
![在这里插入图片描述](https://img-blog.csdnimg.cn/7df1bffcc99e491b905d3521890eabfc.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5pyI5YWJ5LiN5p-T5piv6Z2e,size_20,color_FFFFFF,t_70,g_se,x_16)
- 下载成功
![在这里插入图片描述](https://img-blog.csdnimg.cn/be668a56d15b44f5a36cfc99d39ebd91.png)

- 配置 MapperScan 注解（打开Application类）这是Spring Boot的配置，Spring具体配置请参考官方文档
MapperScan里面的地址怎么填，我按照官方教程里面的```com.baomidou.mybatisplus.samples.quickstart.mapper```然后发现会报错`Process 'command 'C:/Program Files/Java/jdk-17.0.1/bin/java.exe'' finished with non-zero exit value 1` 所以尝试根据目录修改如下(这里我用的还是jdk17，不过换成jdk8也一样会报错)
![在这里插入图片描述](https://img-blog.csdnimg.cn/8ede5053d50e4c038ee5a60a6820118d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5pyI5YWJ5LiN5p-T5piv6Z2e,size_16,color_FFFFFF,t_70,g_se,x_16)
如果根据目录修改不行的话，也可以修改成`com.baomidou.mybatisplus.core.mapper`

> 关于目录下找不到mapper文件的解决方法（网上搜的，仅供参考）
> - gradle默认只会把resource文件夹当成资源文件，如果你的mapper文件放在java目录，则编译后不会被进入编译输出目录，
> - 只需在build.gradle中加入以下 `sourceSets.main.resources.srcDirs = ["src/main/java","src/main/resources"]gradle`就会把java目录也当成资源目录，编译后会将mapper文件输出至目录


- 根据mybatis-plus在2022.1.1的更新记录来看，新增移除 Mapper 相关缓存，支持 GroovyClassLoader 动态注入 Mapper(好吧。。。）

```java
package com.example.demo;

import org.mybatis.spring.annotation.MapperScan;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;


@SpringBootApplication
@MapperScan("com.example.demo.mapper")

public class DemoApplication {

	public static void main(String[] args) {
		SpringApplication.run(DemoApplication.class, args);
	}

}

```
至此，安装配置已经基本完成
> @MapperScan 可以指定要扫描的Mapper类的包的路径

# 注解
新建一个包package，并新建User类
![在这里插入图片描述](https://img-blog.csdnimg.cn/98940162a5c348dbb8d0721d422b68d9.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5pyI5YWJ5LiN5p-T5piv6Z2e,size_20,color_FFFFFF,t_70,g_se,x_16)
- @TableName
描述：表名注解，标识实体类对应的表
使用位置：实体类

```java
package com.example.demo.user;

import com.baomidou.mybatisplus.annotation.TableName;

@TableName(value = "user")
public class User {
    private Long id;
    private String name;
    private Integer age;
    private String email;
}

```
- @TableId
描述：主键注解
使用位置：实体类主键字段

- 更多注释[传送们](https://baomidou.com/pages/223848/#orderby)

# 添加测试依赖
在安装地方配置依赖
Gradle：
```java
implementation group: 'com.baomidou', name: 'mybatis-plus-boot-starter-test', version: '3.5.1'
```
maven：
```java
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter-test</artifactId>
    <version>3.5.1</version>
</dependency>

```
我的build.gradle如下
```java
plugins {
	id 'org.springframework.boot' version '2.6.4'
	id 'io.spring.dependency-management' version '1.0.11.RELEASE'
	id 'java'
}

group = 'com.example'
version = '0.0.1-SNAPSHOT'
sourceCompatibility = '1.8'

repositories {
	mavenCentral()
}

configurations {
	compileOnly {
		extendsFrom annotationProcessor
	}

}

dependencies {
	//spring boot 基础依赖
	implementation 'org.springframework.boot:spring-boot-starter-web'
	testImplementation 'org.springframework.boot:spring-boot-starter-test'

	//mybatis-plus 依赖
	implementation group:'com.baomidou', name: 'mybatis-plus-boot-starter', version: '3.5.1'

	//lombok java库依赖
	compileOnly 'org.projectlombok:lombok'
	annotationProcessor 'org.projectlombok:lombok'

	//mysql数据库依赖
	runtimeOnly 'mysql:mysql-connector-java'

	//其他
	testImplementation('org.springframework.boot:spring-boot-starter-test') {
		exclude group: 'org.junit.vintage', module: 'junit-vintage-engine'
	}
}

tasks.named('test') {
	useJUnitPlatform()
}

```
我的application.properties配置如下
```java
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/test?serverTimezone=GMT%2B8
spring.datasource.username=root
spring.datasource.password=123456
```
我的数据库名字是test，可以根据自己的数据库名字进行修改
（另，如果是jdk8的话可能需要在末尾加上`?serverTimezone=GMT%2B8`就如上所示一致

通过可视化软件确保数据库里面有数据
![在这里插入图片描述](https://img-blog.csdnimg.cn/0084a41ea47c4a7d9370d5bbf1840c2b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5pyI5YWJ5LiN5p-T5piv6Z2e,size_18,color_FFFFFF,t_70,g_se,x_16)

- 然后根据官方的提示进行测试尝试
- 其他测试方法[Spring boot Mybatis-Plus数据库单测实战（三种方式）](https://blog.csdn.net/u012397189/article/details/109288747)
```java
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;

import static org.assertj.core.api.Assertions.assertThat;

@MybatisPlusTest
class MybatisPlusSampleTest {

    @Autowired
    private SampleMapper sampleMapper;

    @Test
    void testInsert() {
        Sample sample = new Sample();
        sampleMapper.insert(sample);
        assertThat(sample.getId()).isNotNull();
    }
}
```


然后实现以下内容，编写实体类User.java，User.java类中内容
```java
import lombok.Data;

@Data
public class User {
    private Long id;
    private String name;
    private Integer age;
    private String email;
}
```

- 官方文档说明了使用lombok简化了代码，而没有安装下载Lombok的话，所声明的类实际上是缺少一些函数的,例如set()、get()等等。

然后，可以直接使用Mybatis-plus让Usermapper继承BaseMapper的接口就可以了
```java
package com.example.demo.user;

import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import org.springframework.stereotype.Repository;

@Repository
public interface Usermapper extends BaseMapper<User> {

}
```

添加测试类，进行功能测试 （根据官方调试也可以）
```java
package com.example.demo;

import com.example.demo.user.User;
import com.example.demo.user.Usermapper;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

import java.util.List;

@SpringBootTest
//@MybatisPlusTest
class DemoApplicationTests {
	@Autowired
	private Usermapper usermapper;

	@Test
	public void contextLoads() {
		List<User> user = usermapper.selectList(null);
		System.out.println(user);
	}

}
```
- 测试结果
![在这里插入图片描述](https://img-blog.csdnimg.cn/5f618ac7dda24e309885d5eb3578e46d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5pyI5YWJ5LiN5p-T5piv6Z2e,size_20,color_FFFFFF,t_70,g_se,x_16)

- 如果运行不成功的话，可以尝试以下方法
- 1.检查实现的实体类是否符合要求，表名字是否符合，内容是否对应
- 2.尝试注释掉官方文档使用的 @MybatisPlusTest
- 3.在mapper的包中的Usermapper的类前面添加@Mapper

我的文件目录截图如下，
![在这里插入图片描述](https://img-blog.csdnimg.cn/29668ca744054edab150d838ce3731a7.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5pyI5YWJ5LiN5p-T5piv6Z2e,size_11,color_FFFFFF,t_70,g_se,x_16)
application.properties
```java
plugins {
	id 'org.springframework.boot' version '2.6.4'
	id 'io.spring.dependency-management' version '1.0.11.RELEASE'
	id 'java'
}

group = 'com.example'
version = '0.0.1-SNAPSHOT'
sourceCompatibility = '1.8'

repositories {
	mavenCentral()
}

configurations {
	compileOnly {
		extendsFrom annotationProcessor
	}

}

dependencies {
	//spring boot 基础依赖
	implementation 'org.springframework.boot:spring-boot-starter-web'
	implementation 'junit:junit:4.13.1'
	testImplementation 'org.springframework.boot:spring-boot-starter-test'

	//mybatis-plus 依赖
	implementation group:'com.baomidou', name: 'mybatis-plus-boot-starter', version: '3.5.1'

	//mybatis-plus 测试依赖
	implementation group:'com.baomidou', name: 'mybatis-plus-boot-starter-test', version: '3.5.1'

	//lombok java库依赖
	compileOnly 'org.projectlombok:lombok'
	annotationProcessor 'org.projectlombok:lombok'

	//mysql数据库依赖
	runtimeOnly 'mysql:mysql-connector-java'

	//其他
	testImplementation('org.springframework.boot:spring-boot-starter-test') {
		exclude group: 'org.junit.vintage', module: 'junit-vintage-engine'
	}

	//代码生成器(新)
	implementation group:'com.baomidou', name: 'mybatis-plus-generator', version: '3.5.2'

	//implementation "io.springfox:springfox-boot-starter:3.0.0"
}

tasks.named('test') {
	useJUnitPlatform()
}

sourceSets.main.resources.srcDirs = ["src/main/java","src/main/resources"]
```

build.gradle如下

```java
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/test?serverTimezone=GMT%2B8
spring.datasource.username=root
spring.datasource.password=123456

#mybatis log
mybatis-plus.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl

```

>`Assert.assertEquals();` 及其重载方法: 
>1. 如果两者一致, 程序继续往下运行. 
>2. 如果两者不一致, 中断测试方法, 抛出异常信息
>3. 在Usermapper文件中的类前面是否添加了`@mapper`注释


注解 ` @MybatisPlusTest` 的作用在于我们可以不用启动整个数据库，仅仅启动我们所需要的模块进行测试

- 可以在application.properties中添加
```properties
#mybatis log
mybatis-plus.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl
```
这样就可以在控制台看到具体SQL和操作

## CRUD 接口
- 通用Service CRUD封装IService接口，进一步封装CRUD采用 `get` 查询单行，`remove`删除`list`集合查询`page`分页前缀命名方式区分Mapper层防止混淆。

- 插入一条记录，我是在测试类当中进行操作的
- 前提：User类中已经通过@data即lombok建立好了User类中一些构造方法等

我的User类
```java
package com.example.demo.user;

import com.baomidou.mybatisplus.annotation.TableField;
import com.baomidou.mybatisplus.annotation.TableName;
import lombok.Data;
import lombok.Getter;
import lombok.Setter;

@Data
@TableName(value = "user")
public class User {
    @Getter
    @Setter
    private Long id;

    @TableField(value = "rname")
    @Getter
    @Setter
    private String name;

    @Setter
    @Getter
    private Integer age;

    @Setter
    @Getter
    private String email;

    public User(long l, String s, int i, String s1) {
        this.id = l;
        this.age = i;
        this.name = s;
        this.email = s1;
    }
}

```

- 我的测试类代码DemoApplicationTests
```java
package com.example.demo;

import com.baomidou.mybatisplus.test.autoconfigure.MybatisPlusTest;
import com.example.demo.user.User;
import com.example.demo.mapper.Usermapper;
import lombok.Getter;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.data.cassandra.DataCassandraTest;
import org.springframework.boot.test.context.SpringBootTest;

import java.util.List;

@SpringBootTest
//@MybatisPlusTest
class DemoApplicationTests {
	@Autowired
	private Usermapper usermapper;

	@Test
	public void contextLoads() {
		List<User> user = usermapper.selectList(null);
		System.out.println(user);
	}
	@Test
	public void testInsert() {
		User user1 = new User(11L, "111", 111, "111@qq.com");
		usermapper.insert(user1);
	}
}
```
运行testInsert可以得到如下结果
![在这里插入图片描述](https://img-blog.csdnimg.cn/496dd0d8ba934a29bb371938aad3bcb7.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5pyI5YWJ5LiN5p-T5piv6Z2e,size_19,color_FFFFFF,t_70,g_se,x_16)
假如想要使用@data的getter和setter去尝试是否能用或者可行
那么我们在user类中建立一个无参构造函数，然后测试类当中这么写
```java
	public void testInsert() {
		User user2 = new User();
		user2.setAge(222);
		user2.setEmail("222@qq.com");
		user2.setId(22L);
		user2.setName("222");
		usermapper.insert(user2);
	}
```
可以得到
![在这里插入图片描述](https://img-blog.csdnimg.cn/714482983cdf4f738ddbfc6c62fce3d2.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5pyI5YWJ5LiN5p-T5piv6Z2e,size_15,color_FFFFFF,t_70,g_se,x_16)
其余方法基本相同。
要注意

 1. 你设置的参数是不是可以重复，如果不能重复，但是你赋值的时候重复了，底下可能会报`Error updating database. Cause: java.sql.SQL`类似错误。
 2. 实体类中要用到的东西你是否声明了，如果没有声明，你却想用，这显然是不可能的，无论是在什么程序当中，都是异想天开的。
 3. 

### save
```java
// 插入一条记录（选择字段，策略插入） 实体对象
boolean save(T entity);
// 插入（批量）实体对象集合
boolean saveBatch(Collection<T> entityList);
// 插入（批量）插入批次数量
boolean saveBatch(Collection<T> entityList, int batchSize);
```
### Remove
```java
// 根据 entity 条件，删除记录、实体对象
boolean remove(Wrapper<T> queryWrapper);
// 根据 ID 删除、实体对象封装操作类
boolean removeById(Serializable id);
// 根据 columnMap 条件，删除记录、实体对象集合
boolean removeByMap(Map<String, Object> columnMap);
// 删除（根据ID 批量删除）、插入批次数量
boolean removeByIds(Collection<? extends Serializable> idList);
```

### update
```java
// 根据 UpdateWrapper 条件，更新记录 需要设置sqlset
boolean update(Wrapper<T> updateWrapper);
// 根据 whereWrapper 条件，更新记录
boolean update(T updateEntity, Wrapper<T> whereWrapper);
// 根据 ID 选择修改
boolean updateById(T entity);
// 根据ID 批量更新
boolean updateBatchById(Collection<T> entityList);
// 根据ID 批量更新
boolean updateBatchById(Collection<T> entityList, int batchSize);
```
### get
```java
// 根据 ID 查询
T getById(Serializable id);
// 根据 Wrapper，查询一条记录。结果集，如果是多个会抛出异常，随机取一条加上限制条件 wrapper.last("LIMIT 1")
T getOne(Wrapper<T> queryWrapper);
// 根据 Wrapper，查询一条记录
T getOne(Wrapper<T> queryWrapper, boolean throwEx);
// 根据 Wrapper，查询一条记录
Map<String, Object> getMap(Wrapper<T> queryWrapper);
// 根据 Wrapper，查询一条记录
<V> V getObj(Wrapper<T> queryWrapper, Function<? super Object, V> mapper);
```

### 省略
QAQ
以上内容实现后，基本入门了，后面的就可以根据官方文档进行实现了，就没有那么多由于版本不同的乱七八糟的错误了。

# 解释
（问题全部以二级标题书写，并非格式错误，而是为了方便查询）
## 1. application.properties和application.yml文件的区别

> 在网上搜索教程的过程中，可以发现许多的教程中编写的都是appication.yml，而从始至终都没有提及application.properties，为此便产生了疑问，二者是否是同一种东西，是否是版本不同的产物？

答案是，相似但不一致

通过短暂接触我们可以发现SpringBoot是经常的通过外部配置去定义属性的。
二者的差别在于加载顺序不同，以及yml分层比properties更为清晰

- 可以使用 @PropertySource 注解加载自定义的 Properties 配置文件，但无法加载自定义的 YAML 文件。
YAML 支持列表的配置，而 Properties 不支持。

原文出自：www.hangge.com  转载请保留原文链接：https://www.hangge.com/blog/cache/detail_2459.html
## 2. 什么是H2数据库？
>H2是一個Java編寫的關係型資料庫，它可以被嵌入Java應用程式中使用，或者作為一個單獨的資料庫伺服器執行。 H2資料庫的前身是 HypersonicSQL，它的名字的含義是 Hypersonic2，但是它的代碼是從頭開始編寫的，沒有使用HypersonicSQL或者HSQLDB的代碼。
>(来源维基百科)

## 3. 关于出现 `Process 'command 'C:/Program Files/Java/jdk-17.0.1/bin/java.exe'' finished with non-zero exit value 1` 类似错误时
解决方法
 1. 可能时由于端口冲突导致的，可以更改端口->打开application.properties,然后添加`server.port=8082`
 2. >This happens when you have DB plugins in your project but you did not specify DB details in application.property file.
就是存在数据库插件，但是你没有进行相应的配置，所导致的错误，所以可以检查下相应配置是否有出现问题
3. 也可能是编译器的问题？，intellij？， File -> invalidate cache.. -> invalidate and restart，清除缓存并重新启动。
4. build.gradle中配置的java版本有问题，打开cmd输入java -version查看java版本，然后更改build.gradle中设置，如图
![在这里插入图片描述](https://img-blog.csdnimg.cn/9aeaf46acbf8473fb7ac5717733095a7.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5pyI5YWJ5LiN5p-T5piv6Z2e,size_15,color_FFFFFF,t_70,g_se,x_16)
5. 由于某种原因，你需要尝试重新构建build.gradle,所以可以考虑点击gradle的刷新

## 4. 什么是代码生成器？
- 代码生成器是生成特定类型的代码或计算机编程语言的工具或资源。 

## 5. 什么是MapperScan？
添加@MapperScan(“com.xxx”)注解以后，com.xxx包下面的接口类，在编译之后都会生成相应的实现类，从而解决了Mapper要逐一进行注释的冗余。
(根据网上信息，@Mapper并不是在编译期间生成实现类，而是在运行时，通过动态代理生成的，[来源](https://blog.csdn.net/manchengpiaoxue/article/details/84937257)）
另，MapperScan还可以同时注解多个包
```
@MapperScan("com.xxx", "com.yyy")
```

## 6. 关于关于IntelliJ IDEA 提示 Execution failed for task ':xxApplication.main()'.的解决办法
[传送门](https://my.oschina.net/zxin/blog/5261165)

## 参考文档
[Mybatis-plus学习笔记](https://www.liuyang19900520.com/subject/layman-starter/layman-starter-01.html)
[官方文档](https://baomidou.com/)
[Mybatis plus入门教程](https://blog.csdn.net/weixin_46085086/article/details/122451131)
