##### [原文](https://blog.csdn.net/dongdong9223/article/details/71425077)

# docker中宿主机与容器（container）互相拷贝传递文件的方法


## 1 从容器拷贝文件到宿主机
拷贝方式为：

docker cp 容器名：容器中要拷贝的文件名及其路径 要拷贝到宿主机里面对应的路径

例如，将容器：

mycontainer

中路径：

/opt/testnew/

下的文件：

file.txt

拷贝到宿主机：

/opt/test/

路径下，在宿主机中执行命令如下：

> docker cp mycontainer:/opt/testnew/file.txt /opt/test/

## 2 从宿主机拷贝文件到容器
拷贝方式为：

docker cp 宿主机中要拷贝的文件名及其路径 容器名：要拷贝到容器里面对应的路径

例如，将宿主机中路径：

/opt/test/

下的文件：

file.txt

拷贝到容器：

mycontainer

的：

/opt/testnew/

路径下，同样还是在宿主机中执行命令如下：

> docker cp /opt/test/file.txt mycontainer:/opt/testnew/

## 3 不管容器有没有启动，拷贝命令都会生效

需要注意的是，不管容器有没有启动，拷贝命令都会生效。

