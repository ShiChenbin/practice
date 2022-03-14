# 捕获protocol modbus_IEC104_cdt工具包发出的信息，并对其分析

工具包来源：https://gitee.com/weiyigulu_admin/protocol
- 超时定义
![image](https://user-images.githubusercontent.com/49110003/158120738-e7695e4d-ede5-4ead-b138-f21f1c2b9d55.png)
- S帧，I帧，U帧
S帧：用于确认的帧。比如说，通信过程，我发了一帧给被控对象，它就用这个S帧的对应格式进行应答。
I帧：帧的结果帧带有功能性，比如说，召取具体的数据。
U帧：测试帧，用于测试通信通道是否正常，类似于心跳帧。

- APCI:应用规约控制信息,就是用于说明这一帧如何进行控制，是定长帧，还是变长帧，这个APCI头是如何防止报文丢失和报文重复传送的。
ASDU：应用服务数据单元，就是说传输帧的具体功能的，比如说是一级遥测，一级遥新，二级遥信，二级遥测，全召，授时等等。
APDU：APCI+ASDU

# 主要分析的IEC-104协议下发出的包
工具是基于MAVEN下的，gradle不知道如何调取这个工具的依赖【待解决】

该工具内附有测试软件包，可以先用其进行测试。

项目打开路径：文件 file -> file open -> open protocol-master.pom.xml -> open as project

我们分析的是IEC-104协议，所以我们启动对应软件包下的测试类就可以了，打开后目录如下，

![image](https://user-images.githubusercontent.com/49110003/157814076-a1d7763f-2595-4aab-b672-fff679b276ab.png)

# 根据项目目录分析项目各个类的作用

annotation
AsduType.java（APDU应用规约数据单元：默认值、类型id、是否优先判断函数、名字构建器)
apdumodel （APDI模型）
- Apdu.java()（本端发送序列号、本端接受序列号、本端的类型、ASDU应用服务数据单元，iec 104 builder，记录传输通道、枚举APCI类型【i帧、s帧、u帧】、loadByteBuf【读取字节流，将数据转化为APDU】）
- Asdu.java（asdu实体）
- Cot.java（传输原因）
- package-info.java
- Vsq.java

asdudataframe（asdu数据框架）
container（容器）
excepttion（异常处理）
nettyconfig（网络配置）
util

test
java
- ClientTest（客户端测试）新建一个ip和该ip的端口（127.0.0.1:2404），设置共享ip（127.0.0.1）
- MasterTest（主机测试）生成一个主机端口（也是127.0.0.1:2404）
- NettyClient （网络客户端） 绑定线程池->指定使用的通道channel->绑定客户端连接时触发的操作（服务器异步创建绑定）->最后关闭服务器通道
接收服务端发送来的消息channelRead0 + 与服务器建立连接channelActive + 与服务器断开连接channelInactive + 异常处理exceptionCaught
- - ClientHandler4（客户端处理程序）
- - NettyClient （客户端网络）
- Server4（测试客户端）
- - Server4 （测试客户端）辅助启动类->设置线程池->设置socket工厂（套接口） ->设置管道工厂 —>设置TCP参数
- - ServerHandler4 读取客户端发送的数据
- SlaveTest 附属测试
- TestTotals 完全测试
            

注意端口都是2404的，即port:2404
(如果抛出error很有可能是端口不一致导致的，原工具包已经配置好，一般不会出现)

- 准备wireshark

打开上述目录下的test包

启动顺序应该是
run

//NettyClient和Server4不能同时启动？否侧会抛出地址已经被使用的报错，例如`Address already in use: bind`
- SlaveTest （发出U帧，启动命令）
- server4 （显示sever start ...... ）
- MasterTest （）
- ClientTest

然后抓包，然后找到iec104的项

![image](https://user-images.githubusercontent.com/49110003/157815696-01e4c7f0-a01e-4818-b6f2-0e3b2b38251a.png)

！[图片] (https://user-images.githubusercontent.com/49110003/157816384-f2c73fbf-3cd9-4245-8de6-f287f9bf90d5.png)

- 在 APDU 应用规约数据单元中，APCI应用规约控制信息中的启动字符为68H，可以发现68H恰好对应的是start，符合iec104协议中的启动字符为（68H）
- 然后APDU的长度是4，也就是ASDU+4，那么可以得出ASDU = 0
- 剩下的是07 00 00 00，这部分是（控制域1+控制域2+控制域3+控制域4）+（ASDU应用服务数据单元【可选】）
- Type: I（0x00）表示I帧， I帧中的控制域1与控制域2为发送序号，控制域3与控制域4为接收序号。
- DEC = 1，HEX = 01，含义单点信息，说明不带时标的单点遥信，每个遥信占一个字节

干脆筛完全部看IEC部分
![image](https://user-images.githubusercontent.com/49110003/157819171-d968dbd7-56c0-4d61-a61e-90d7818acbd7.png)

定睛一分析，那么必然是只有发送方而接受方无作为
![image](https://user-images.githubusercontent.com/49110003/157819533-6b0a8397-5bec-4cf9-8202-acd70d31508c.png)

注意接受方的端口，我是在本机上进行的所以接受方的端口应该是在本机上127.0.0.1
