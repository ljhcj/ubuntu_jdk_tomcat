# 默认ubuntu server长期支持版本，当前版本14.04
# 此Dockerfile生成Ubuntu14.04+Jdk1.8+Tomcat8的基础镜像
FROM registry.cn-hangzhou.aliyuncs.com/gf/ubuntu-14.04:latest

# 作者
MAINTAINER  "sh_ljhcj@163.com" 2017-2-10

# 安装更新、安装wget和ssh工具、修改系统时区
RUN apt-get update \
    && apt-get -y install wget vim \
	  && /bin/cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && echo 'Asia/Shanghai' >/etc/timezone

#把java与tomcat添加到容器中
ADD jdk-8u111-linux-x64.tar.gz  /usr/local/
ADD apache-tomcat-8.5.4.tar.gz /usr/local/

#配置java与tomcat环境变量
ENV JAVA_HOME /usr/local/jdk1.8.0_111
ENV JRE_HOME $JAVA_HOME/jre
ENV CLASSPATH .:$JAVA_HOME/lib:$JRE_HOME/lib
ENV PATH $PATH:$JAVA_HOME/bin

ENV CATALINA_HOME /usr/local/apache-tomcat-8.5.4
ENV CATALINA_BASE /usr/local/apache-tomcat-8.5.4
ENV PATH $PATH:$JAVA_HOME/bin:$CATALINA_HOME/lib:$CATALINA_HOME/bin

#容器运行时对外开放的端口
EXPOSE 8080 22

#工作目录 /usr/local/apache-tomcat-8.5.4/bin
CMD ["/usr/local/apache-tomcat-8.5.4/bin/catalina.sh","run"]
