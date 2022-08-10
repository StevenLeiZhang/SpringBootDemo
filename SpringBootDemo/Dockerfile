FROM maven:3.8.6-jdk-8 AS build
COPY . .

RUN mvn clean package


# 构建一个基于Java8的镜像
FROM java:8

ARG site
ARG nextSite=""


ENV envSite=$site
ENV envNextSite=$nextSite


# 镜像构建时执行命令
#RUN bash -c 'touch /hello1-0.0.1-SNAPSHOT.jar'
#RUN base -c 'curl  -s "http://sgm-server-headless.jdd-paas.svc.cluster.local:8042/local/probe/java/script?app=hello1"  -o /tmp/sgm_java.sh && source /tmp/sgm_java.sh '

RUN mkdir -p /data/hello1
#COPY script_start.sh /data/hello1/
COPY --from=build script_start.sh /data/hello1/
# 拷贝文件到Docker容器根目录
#COPY ./target/hello1-0.0.1-SNAPSHOT.jar /data/hello1/
COPY --from=build ./target/com.xiaour.spring.boot-1.0-SNAPSHOT.jar /data/hello1/
RUN chmod +x /data/hello1/script_start.sh
RUN chmod +x /data/hello1/com.xiaour.spring.boot-1.0-SNAPSHOT.jar
RUN mkdir -p "/export/log/hello1"

#RUN apt-get install jq
WORKDIR /data/hello1
# 声明端口
EXPOSE 8080
# 容器启动时执行命令
#ENTRYPOINT ["java","$SGM_OPTS","-jar","/hello1-0.0.1-SNAPSHOT.jar"]
#ENTRYPOINT ./script_start.sh  $envSite  $envNextSite
ENTRYPOINT ["java", "-jar", "hello1-0.0.1-SNAPSHOT.jar"]