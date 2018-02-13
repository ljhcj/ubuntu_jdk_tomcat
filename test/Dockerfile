FROM registry.cn-qingdao.aliyuncs.com/zyyl/ubuntu14.04:latest

ENV     JAVA_VERSION_MAJOR=8 \
        JAVA_VERSION_MINOR=102 \
        JAVA_VERSION_BUILD=14 \
        JAVA_PACKAGE=jdk
ENV     DOWNLOAD_URL=http://download.oracle.com/otn-pub/java/jdk/${JAVA_VERSION_MAJOR}u${JAVA_VERSION_MINOR}-b${JAVA_VERSION_BUILD}/${JAVA_PACKAGE}-${JAVA_VERSION_MAJOR}u${JAVA_VERSION_MINOR}-linux-x64.tar.gz

RUN apt-get update \
    && apt-get -y install wget vim gzip curl \
    && /bin/cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && echo 'Asia/Shanghai' >/etc/timezone \
    && apt-get autoclean && apt-get --purge -y autoremove \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# 把java与tomcat添加到容器中
RUN wget -P /usr/local/ "${DOWNLOAD_URL}" \
    && wget -P /usr/local/ http://download.zhongyinginfo.com/soft/apache-tomcat-7.0.69.tar.gz \
    && cd /usr/local/ && tar -zxvf jdk-7u79-linux-x64.tar.gz && tar -zxvf apache-tomcat-7.0.69.tar.gz && rm -rf *.tar.gz \
    && rm -rf /usr/local/apache-tomcat-7.0.69/conf/server.xml \
    && cd /usr/local/apache-tomcat-7.0.69/webapps && rm -rf *

# 配置java与tomcat环境变量
ENV JAVA_HOME /usr/local/jdk1.7.0_79
ENV JRE_HOME $JAVA_HOME/jre
ENV CLASSPATH .:$JAVA_HOME/lib:$JRE_HOME/lib
ENV PATH $PATH:$JAVA_HOME/bin

ENV CATALINA_HOME /usr/local/apache-tomcat-7.0.69
ENV CATALINA_BASE /usr/local/apache-tomcat-7.0.69
ENV PATH $PATH:$JAVA_HOME/bin:$CATALINA_HOME/lib:$CATALINA_HOME/bin

# 修改tomcat内存大小并设置字符编码格式支持UTF8和日志格式
RUN sed -i '/^\# OS specific/i \JAVA_OPTS="-Dfile.encoding=UTF-8 -Dsun.jnu.encoding=UTF-8 -server -Xms1024m -Xmx2048m -XX:PermSize=256m -XX:MaxPermSize=512m -XX:MaxNewSize=512m"' /usr/local/apache-tomcat-7.0.69/bin/catalina.sh \
    && apt-get autoclean && apt-get --purge -y autoremove \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
ADD server.xml  /usr/local/apache-tomcat-7.0.69/conf/

# 容器运行时对外开放的端口
EXPOSE 8080

# 工作目录 /usr/local/apache-tomcat-7.0.69/bin
CMD ["/usr/local/apache-tomcat-7.0.69/bin/catalina.sh","run"]
