# 默认ubuntu server长期支持版本，当前版本14.04
# 此Dockerfile生成Ubuntu14.04+Jdk1.8+Tomcat8的基础镜像
FROM registry.cn-hangzhou.aliyuncs.com/gf/ubuntu-14.04:latest

# 作者
MAINTAINER  "sh_ljhcj@163.com" 2017-03-30

# 安装更新、安装wget和ssh工具、修改系统时区
RUN apt-get update \
    && apt-get -y install wget vim \
    && /bin/cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && echo 'Asia/Shanghai' >/etc/timezone

#把java与tomcat添加到容器中
RUN wget -P /usr/local/ http://download.zhongyinginfo.com/soft/jdk-8u111-linux-x64.tar.gz \
    && wget -P /usr/local/ http://download.zhongyinginfo.com/soft/apache-tomcat-8.5.4.tar.gz \
    && cd /usr/local/ && tar -zxvf jdk-8u111-linux-x64.tar.gz && tar -zxvf apache-tomcat-8.5.4.tar.gz && rm -rf *.tar.gz

#配置java与tomcat环境变量
ENV JAVA_HOME /usr/local/jdk1.8.0_111
ENV JRE_HOME $JAVA_HOME/jre
ENV CLASSPATH .:$JAVA_HOME/lib:$JRE_HOME/lib
ENV PATH $PATH:$JAVA_HOME/bin

ENV CATALINA_HOME /usr/local/apache-tomcat-8.5.4
ENV CATALINA_BASE /usr/local/apache-tomcat-8.5.4
ENV PATH $PATH:$JAVA_HOME/bin:$CATALINA_HOME/lib:$CATALINA_HOME/bin

# 修改tomcat内存大小并设置字符编码格式支持UTF8
RUN sed -i '73s/redirectPort="8443"/ redirectPort="8443" URIEncoding="UTF-8"/' /usr/local/apache-tomcat-8.5.4/conf/server.xml \
    && sed -i 's/pattern="%h %l %u %t &quot;%r&quot; %s %b" \/>/pattern="[%a] [%u] [%t] ['%r'] [%s] [%B] [%{X-Forwarded-For}i] [%{Referer}i] [%{User-Agent}i]" \/>/1' /usr/local/apache-tomcat-8.5.4/conf/server.xml \
    && sed -i '/^\# OS specific/i \JAVA_OPTS="-Dfile.encoding=UTF-8 -Dsun.jnu.encoding=UTF-8 -server -Xms1024m -Xmx1024m  -XX:MaxNewSize=512m"' /usr/local/apache-tomcat-8.5.4/bin/catalina.sh

#容器运行时对外开放的端口
EXPOSE 8080

#工作目录 /usr/local/apache-tomcat-8.5.4/bin
CMD ["/usr/local/apache-tomcat-8.5.4/bin/catalina.sh","run"]
