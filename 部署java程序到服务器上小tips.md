- 前言

需要部署多个jar包到服务器上，使用xshell部署

- 打开服务器
- 打开放置jar包的文件夹(根据自身情况)

```shell
cd /usr/local/src
```

- 查看jar包是否在运行(name放置jar包的部分名字即可)

```shell
ps -ef|grep name
```

ps命令的输出格式

```shell
USER PID %CPU %MEM VSZ RSS TTY STAT START TIME COMMAND
```

- 然后杀掉对应的pid

```shell
kill pid
```

- 检查是否杀掉

```shell
ps -ef|grep name
```

- 将你的jar包放置到该文件夹下，然后启动（如果原有直接复制上去覆盖）使用nohup命令。

**nohup** 英文全称 no hang up（不挂起），用于在系统后台不挂断地运行命令，退出终端不会影响程序的运行。

**nohup** 命令，在默认情况下（非重定向时），会输出一个名叫 nohup.out 的文件到当前目录下，如果当前目录的 nohup.out 文件不可写，输出重定向到 **$HOME/nohup.out** 文件中。

nohup 一定要在指定目录下，不然找不到需要启动的jar包，你在a文件夹下肯定启动不了b文件夹里面的程序。

语法格式

```shell
nohup Command [ Arg … ] [　& ]
```

一般使用 (第一个xms512是最小分配内存，第二个是最大分配内存)

```shell
nohup java -jar -Xms512M -Xmx1024M name.jar &
```

- ls 命令用于显示指定工作目录下之内容

**参数** :

-a 显示所有文件及目录 (**.** 开头的隐藏文件也会列出)

-l 除文件名称外，亦将文件型态、权限、拥有者、文件大小等资讯详细列出

-r 将文件以相反次序显示(原定依英文字母次序)

-t 将文件依建立时间之先后次序列出

-A 同 -a ，但不列出 "." (目前目录) 及 ".." (父目录)

-F 在列出的文件名称后加一符号；例如可执行档则加 "*", 目录则加 "/"

-R 若目录下有文件，则以下之文件亦皆依序列出



- cat nohup.out 显示下nohup.out里面的内容，也是之前nohup的记录，看下是否启动成功了
- pwd 可以显示当前路径



# 通过.sh文件快捷部署jar包到服务器上

每次都要杀死pid和启动服务器，未免太过麻烦，那直接写一个脚本文件，每次运行这个文件就行，就能便利许多。

- 在合适的文件夹下创建脚本文件

``` shell
vim start.sh
```

内容如下(三个jar包分别放置在a1、a2、a3文件夹下)

睡眠和输出提示可以根据需要删除和添加，该start.sh就完成了多个进程删除，再部署的功能。

```shell
array=(name1.jar name2.jar name3.jar)
for i in ${array[@]}
do
        PID=$(ps -ef | grep $i | grep -v grep | awk '{ print $2 }')
        if [ -z "$PID" ]
        then
                echo Application is already stopped
        else
                echo kill $PID
                kill -9  $PID
        fi
done

cd /usr/local/src/jars/a1
nohup java -jar -Xms512M -Xmx1024M name1.jar &
sleep 10s
cd /usr/local/src/jars/a2
nohup java -jar -Xms512M -Xmx1024M name2.jar &
sleep 10s
cd /usr/local/src/jars/a3
nohup java -jar -Xms512M -Xmx1024M name3.jar &
echo "finish"
```

- 授予sh文件权限

``` shell
chmod 777 start.sh
```

- 执行sh文件

方法一 本身目录下运行

进入 cd /home/workwen文件下, 执行 

```shell
./start.sh
```

命令会在当前目录下创建一个“test”目录。

方法二 绝对路劲运行, 执行 

```shell
/home/work/start.sh
```

方法三 本身目录下运行

```shell
sh start.sh
```

