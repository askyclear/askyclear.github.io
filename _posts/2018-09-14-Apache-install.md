---
layout: post
title: Apache install
date: 2018-09-14 10:00:00 -0600
---
아파치 2.4 설치 및 톰캣 



# apache-2.4.x 버전 설치

#### 조건 

1. 아파치 2.0을 설치할 때는 apr, apri-util을 설치하지 않았지만 2.4 이상에서는 apr, apr-util이 없기 때문에 별도로 설치해야함.

2. 아파치 2.4를 설치할때에 pcre를 설치해야함.

#### 설치과정
```
#https://sourceforge.net/projects/pcre/files/pcre/ 에서 pcre 버전 보고아래와 같이 설치 파일 다운로드
wget https://sourceforge.net/projects/pcre/files/pcre/8.42/pcre-8.42.tar.gz/download
tar xvfz download 
cd pcre-8.36
./configure --prefix=/usr/local
make
make install

#https://httpd.apache.org/download.cgi 에서 아파치 버전 보고 아래와 같이 설치 파일 다운로드
wget http://mirror.navercorp.com/apache//httpd/httpd-2.4.34.tar.gz
wget http://apache.mirror.cdnetworks.com//apr/apr-1.6.3.tar.gz
wget http://mirror.apache-kr.org//apr/apr-util-1.6.1.tar.gz

# 압축해제 및 설치
tar xvfz httpd-2.4.34.tar.gz
tar xvfz apr-1.6.3.tar.gz
tar xvfz apr-util-1.6.1.tar.gz

# apr을 아파치의 srclib 디렉토리 안으로 이동
mv apr-1.6.3 httpd-2.4.34/srclib/apr
mv apr-util-1.6.1 httpd-2.4.34/srclib/apr-util

cd httpd-2.4.34
# 아파치 configure 설정 후 아파치 설치
./configure \
--prefix=/home/apache2.4.33 \
--with-included-apr \
--with-pcre=/usr/local/bin/pcre-config
make
make install

```

설치과정의 문제인지 pcre를 인식 못한 경우가 있기에 pcre-dev를 설치함으로서 해결

```
yum pcre-dev install
```

출처 http://victorydntmd.tistory.com/220



## apache 서버와 톰켓 연동

appache의 httpd.conf 에서 mod_jk를 로드한다.

mod_jk가 없을 경우

```
wget http://mirror.apache-kr.org/tomcat/tomcat-connectors/jk/tomcat-connectors-1.2.44-src.tar.gz
tar zxvf tomcat-connectors-1.2.44-src.tar.gz
cd tomcat-connectors-1.2.44-src/native

./buildconf.sh
./configure --with-apxs=/home/apache/bin/apxs
make
make install
```

이후 아래와 같이 httpd.conf에 아래와 같이 설정

``` 
<IfModule mod_jk.c>
# 어떤 경로로 요청이 오면 어디로 보낼것인가
JkMount /*.jsp tomcat
JkMount /*.nhn tomcat
JkMount /jkmanager/* jkstatus
JkMountCopy All
<Location /jkmanager/>
        JkMount jkstatus
        Require ip 127.0.0.1
</Location>
#workers.properties 파일 위치
JkWorkersFile "/home/apache/conf/workers.properties"
JkLogFile "| /home/apache/bin/rotatelogs -l /home/logs/apache/mod_jk.log.%y%m%d 86400 "
JkLogLevel error
JkLogStampFormat "[%a %b %d %H:%M:%S %Y] "
JkShmFile  "/home/apache/mod_jk.shm"
</IfModule>
```

### /home/apache/conf/workers.properties

```
worker.list=tomcat

worker.tomcat.type=ajp13
# 어떤 호스트로 연결 
worker.tomcat.host=127.0.0.1
# 톰켓 연동 포트
worker.tomcat.port=8009
worker.tomcat.retries=1
#worker.tomcat.connect_timeout=시간
#worker.tomcat.prepost_timeout=시간
#worker.tomcat.socket_timeout=시간
#worker.tomcat.connection_pool_timeout=시간
#worker.tomcat.reply_timeout=시간

worker.list=jkstatus
worker.jkstatus.type=status
```
