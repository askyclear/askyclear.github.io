---
layout: post
title: Apache httpd.conf
date: 2018-09-10 10:40:00 -0600
---
아파치 설정 파일 정리

![_config.yml]({{ site.baseurl }}/images/config.png)

# httpd.conf
### ServerRoot path 
 아파치 서버가 설치된 경로
```
ServerRoot /home/apache
```

### DocumentRoot path
웹상에서 최상위 폴더가 되는 OS상의 절대 경로
```
DocumentRoot /home/workspace/
```

### \<Directory path\>
path 디렉토리에 대한 제어

#### - Options
 * FollowSymLinks : 심볼릭링크에 대한 허용 여부
 * MultiViews : 클라이언트의 요청에 따라 적절하게 페이지 Show
 * None : 모든설정 거부
 * All : MultiViews를 제외한 옵션
#### - AllowOverride
클라이언트의 디렉토리 접근 제어
 * None : AllowOverride off
 * All : AccessFileName 지시자로 설정한 파일에 대해 민감하게 반응

### DirectoryIndex 파일이름 
 파일 이름을 명시하지 않고 디렉토리에 접근할 경우 자동으로 보여줄 파일 이름 설정
```
DirectoryIndex index.jsp
```

### ErrorDocument
아파치 서버의 에러페이지에 출력할 텍스트나 문서를 정의 
ex) ErrorDocument 404 /errorpage/notfound.html
최상위 경로('/')는 DocumentRoot를 의미

출처: http://mosei.tistory.com/entry/apache-httpdconf-설정-및-설명?category=582083 [씹어먹는 블로그]

# httpd-ssl.conf

https로 접속시 바로 80포트로 연결되는 것이 아닌 443포트로 연결이 들어온다.
이때문에 ssl적용시 vHosts에서 설정했던것 처럼 DocumentRoot등을 설정해 준다.

### SSLCertificateFile path
인증서 파일 등록
```
SSLCertificateFile "/home/apache/conf/ssl/ZZ.pem"
```
### SSLCertificateKeyFile path
Chain CA 인증서 파일
```
SSLCertificateKeyFile "/home/apache/conf/ssl/YY.key"
```
### SSLCertificateChainFile path
```
SSLCertificateChainFile "/home/apache/conf/ssl/XX.crt"
```



# httpd-vhosts.conf

### hosts 파일
 운영 체제가 호스트 이름(도메인주소)을 IP주소에 매핑할 때 사용하는 컴퓨터 파일
 보통 브라우저에서 호스트 이름(도메인주소)로 서버에 요청을 할 때 DNS 서버를 통해 ip를 얻은 후 서버에 접근하지만 hosts에 등록을 하면 DNS 서버를 거치지 않고 직접적으로 연결함
윈도우 10에서는 	%SystemRoot%\System32\drivers\etc\ 에 hosts 파일이 존재

### 리눅스 가상호스트(vhost)
하나의 웹서버에서 2개 이상의 여러 개의 웹서버를 운영하는것
#### 운영방법
 * 도메인네임 기반 가상 호스트
```
<VirtualHost 192.168.1.1>
ServerName	alpha.linux.org
...
</VirtualHost>
<VirtualHost 192.168.1.1>
ServerName delta.linux.org
...
```
 * IP 주소 기반 가상 호스트
 ```
<VirtualHost 192.168.1.1>
ServerName	alpha.linux.org
...
</VirtualHost>
<VirtualHost 192.168.1.2>
ServerName delta.linux.org
 ```

 * 포트 기반 가상 호스트
 ```
 <VirtualHost *:80>
 ...
 ```
