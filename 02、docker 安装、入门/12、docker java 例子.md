## [原文](https://www.qikegu.com/docs/3033)

## [Docker Java应用程序示例](https://www.yiibai.com/docker/docker-java-example.html)

## [Run Java command line in Docker containers](https://gordonlesti.com/run-java-command-line-in-docker-containers/)


# Docker Java 例子

> 注意这个文件，在docker 设置好的 file sharing 里面设置的文件夹

## 1. 创建项目目录
我们会把这个项目的相关文件，集中放到一个目录docker-java
```shell script
[root@docker demo]# mkdir docker-java
```

## 2. 创建Java文件
在docker-java目录下，创建一个Java文件：
```java
public class HelloWorld {
    public static void main(String[] args){
        System.out.println("This is java docker Hello World \n");
    }
}
```

## 3. 创建Dockerfile
创建Java文件之后，我们需要创建一个Dockerfile，其中包含了Docker的指令。
在docker-java目录下创建Dockerfile，文件名必须是Dockerfile。

Dockerfile
```dockerfile
FROM java:8
COPY ../07 /var/www/java
WORKDIR /var/www/java
RUN javac HelloWorld.java
CMD ["java", "HelloWorld"]
```
所有指令都大写，这是惯例。

现在docker-java目录下有2个文件：
```shell script
[root@docker docker-java]# ls
Dockerfile  HelloWorld.java
```

## 4. 构建 Docker 镜像
切换到docker-java目录，运行docker build -t hello-docker-java . 命令，构建Docker镜像。
Docker镜像可以任意取名，此处命名为 hello-docker-java
```shell script
[root@docker docker-java]# docker build -t hello-docker-java .
Sending build context to Docker daemon  3.072kB
Step 1/5 : FROM java:8
 ---> d23bdf5b1b1b
Step 2/5 : COPY . /var/www/java
 ---> Using cache
 ---> 7f24b5fc6fb6
Step 3/5 : WORKDIR /var/www/java
 ---> Using cache
 ---> 2eacd7222454
Step 4/5 : RUN javac HelloWorld.java
 ---> Using cache
 ---> bf254a2eec11
Step 5/5 : CMD ["java", "HelloWorld"]
 ---> Using cache
 ---> 1842ec92df2d
Successfully built 1842ec92df2d
Successfully tagged hello-docker-java:latest
```
查看镜像
```shell script
[root@docker docker-java]# docker images
REPOSITORY                TAG                 IMAGE ID            CREATED             SIZE
hello-docker-java         latest              1842ec92df2d        13 minutes ago      643MB
<none>                     <none>              327ab0702d14        14 minutes ago      643MB
...
```
这里，最后使用docker images查看镜像，可以看到构建镜像成功。接下来就可以运行镜像了。

## 5. 运行 Docker 镜像
执行docker run hello-docker-java 命令运行镜像：
```shell script
[root@docker docker-java]# docker run hello-docker-java
This is java docker Hello World
```
可以看到，hello-docker-java 镜像成功运行，输出了一条信息。


## ps 如果遇到不输出的

比如我的镜像是这个。。。。死活就是没有输出出来
```dockerfile
FROM openjdk:8-jdk-alpine
COPY ../04 /var/www/java
WORKDIR /var/www/java
RUN javac TimeZoneExample.java
CMD ["java", "TimeZoneExample"]

FROM alpine:3.6
RUN apk update && apk add --no-cache tzdata
ENV TZ=Asia/Shanghai

RUN echo "Asia/Shanghai" > /etc/timezone
```

TimeZoneExample.java
```Java
import java.time.ZoneId;


public class TimeZoneExample {
    public static void main(String[] args) {
        System.out.println("ZoneId={}"+ZoneId.systemDefault().getId());
    }
}
```

需要调整下  dockerfile文件

```dockerfile

FROM alpine:3.6
RUN apk update && apk add --no-cache tzdata
ENV TZ=Asia/Shanghai

FROM openjdk:8-jdk-alpine
COPY . /var/www/java
WORKDIR /var/www/java
# 注意这里的顺序
RUN echo "Asia/Shanghai" > /etc/timezone
RUN javac TimeZoneExample.java
CMD ["java", "TimeZoneExample"]
```
如果还有MYSQL 之类的时区也记得设置



## 下面这种方式不需要build 镜像

> 如果镜像不存在，会自动去拉取镜像
```shell script
docker run -v "$PWD":/usr/src/hello-docker -w /usr/src/hello-docker openjdk:8 javac HelloWorld.java

docker run -v "$PWD":/usr/src/hello-docker -w /usr/src/hello-docker openjdk:8 java HelloWorld
This is java docker Hello World
```
/usr/src/hello-docker 这个文件夹路径你需要注意，
这里应该是你 docker配置的  file sharing 里面设置的
