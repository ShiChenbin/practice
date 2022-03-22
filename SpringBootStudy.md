# 引导类
![image](https://user-images.githubusercontent.com/49110003/159426042-f2bfd3a3-fde7-4ef0-a966-ca29888fb4a3.png)

# RESTFUL 入门
![image](https://user-images.githubusercontent.com/49110003/159428299-1840bde2-af2e-44c8-8193-6d3bfd75e698.png)  
ResquestMapping

# SpringBoot项目分层
（1）Controller层：控制层

Controller层负责具体的业务模块流程的控制。Controller层负责前后端交互，接受前端请求，调用Service层，接收Service层返回的数据，最后返回具体的页面和数据到客户端。

（2）Service层：业务层

Service层负责业务模块的逻辑应用设计。先设计放接口的类，再创建实现的类，然后在配置文件中进行配置其实现的关联。Service层调用Dao层接口，接收Dao层返回的数据，完成项目的基本功能设计。封装Service层的业务逻辑有利于业务逻辑的独立性和重复利用性。

（3）Dao层：数据访问层

Dao层负责与数据库进行交互。Dao层封装对于数据库的增删改查，不涉及业务逻辑，只是达到按某个条件对数据库中的数据进行某一具体操作的要求。（有的项目中也将Dao层写为Mapper层）

（4）Domain层：数据表对象层

Domain层定义数据库中存储的实体类。（有的项目也将Domain层写为Entity层或Model层）

（5）Config层：配置类层

Config层定义一些与配置有关的类。

# 提示消失的解决办法

![image](https://user-images.githubusercontent.com/49110003/159442599-49da99ce-b305-4e50-9513-f860ea11c2bf.png)
