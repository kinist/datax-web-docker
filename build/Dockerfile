FROM ubuntu:20.04

LABEL vendor=Gavin\ Pangu \
      maintainer="kinist@msn.com" \
      version="1.0.0" \
      buildtime="20220115" \
      description="DataX"

ARG MAVEN_VERSION=3.8.4
ARG DATAX_WEB_VERSION=2.1.2
ARG DB_HOST=mysql
ARG DB_PORT=3306
ARG DB_USERNAME=root
ARG DB_PASSWORD=mypassword
ARG DB_DATABASE=datax_web

ENV DEBIAN_FRONTEND=noninteractive

RUN sed -i "s/archive.ubuntu.com/mirrors.aliyun.com/g" /etc/apt/sources.list && \
    sed -i "s/security.ubuntu.com/mirrors.aliyun.com/g" /etc/apt/sources.list

RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y --no-install-recommends wget vim git tzdata openjdk-8-jdk-headless python net-tools inetutils-ping && \
    apt-get autoremove -y && \
    rm -rf /var/lib/apt/lists/*

RUN wget https://dlcdn.apache.org/maven/maven-3/${MAVEN_VERSION}/binaries/apache-maven-${MAVEN_VERSION}-bin.tar.gz -O /opt/maven.tar.gz && \
    cd /opt && tar -zxvf maven.tar.gz && rm -f maven.tar.gz
ENV PATH=${PATH}:/opt/apache-maven-${MAVEN_VERSION}/bin

# COPY datax.tar.gz /opt
# COPY config.json /opt
RUN wget http://datax-opensource.oss-cn-hangzhou.aliyuncs.com/datax.tar.gz -O /opt/datax.tar.gz && \
    cd /opt && tar -zxvf datax.tar.gz && rm -f datax.tar.gz && \
    rm -f ._* && \
    rm -f datax/._* && \
    rm -f datax/bin/._* && \
    rm -f datax/conf/._* && \
    rm -f datax/job/._* && \
    rm -f datax/lib/._* && \
    rm -rf datax/log && \
    rm -rf datax/log_perf && \
    rm -f datax/plugin/._* && \
    rm -f datax/plugin/reader/._* && \
    rm -f datax/plugin/writer/._* && \
    rm -f datax/script/._* && \
    rm -f datax/tmp/._*

RUN wget https://github.com/WeiYe-Jing/datax-web/archive/refs/tags/v-${DATAX_WEB_VERSION}.tar.gz -O /opt/datax-web-v-${DATAX_WEB_VERSION}.tar.gz && \
    cd /opt && tar -zxvf datax-web-v-${DATAX_WEB_VERSION}.tar.gz && rm -f datax-web-v-${DATAX_WEB_VERSION}.tar.gz

RUN cd /opt/datax-web-v-${DATAX_WEB_VERSION} && mvn clean install && rm -rf /root/.m2 && \
    mv /opt/datax-web-v-${DATAX_WEB_VERSION}/build/datax-web-${DATAX_WEB_VERSION}.tar.gz /opt && rm -rf /opt/datax-web-v-${DATAX_WEB_VERSION} && \
    cd /opt && tar -zxvf datax-web-${DATAX_WEB_VERSION}.tar.gz && rm -f datax-web-${DATAX_WEB_VERSION}.tar.gz && \
    cd /opt/datax-web-${DATAX_WEB_VERSION}/bin && chmod 755 *.sh && ./install.sh --force && \
    ## 修改数据库信息
    sed -i 's/#DB_HOST=/DB_HOST='${DB_HOST}'/g' /opt/datax-web-${DATAX_WEB_VERSION}/modules/datax-admin/conf/bootstrap.properties && \
    sed -i 's/#DB_PORT=/DB_PORT='${DB_PORT}'/g' /opt/datax-web-${DATAX_WEB_VERSION}/modules/datax-admin/conf/bootstrap.properties && \
    sed -i 's/#DB_USERNAME=/DB_USERNAME='${DB_USERNAME}'/g' /opt/datax-web-${DATAX_WEB_VERSION}/modules/datax-admin/conf/bootstrap.properties && \
    sed -i 's/#DB_PASSWORD=/DB_PASSWORD='${DB_PASSWORD}'/g' /opt/datax-web-${DATAX_WEB_VERSION}/modules/datax-admin/conf/bootstrap.properties && \
    sed -i 's/#DB_DATABASE=/DB_DATABASE='${DB_DATABASE}'/g' /opt/datax-web-${DATAX_WEB_VERSION}/modules/datax-admin/conf/bootstrap.properties && \
    ## 修改DataX启动Python信息
    sed -i 's/PYTHON_PATH=/PYTHON_PATH=\/opt\/datax\/bin\/datax.py/g' /opt/datax-web-${DATAX_WEB_VERSION}/modules/datax-executor/bin/env.properties

WORKDIR /opt/datax-web-${DATAX_WEB_VERSION}/bin

# ENTRYPOINT [ "./start-all.sh" ]