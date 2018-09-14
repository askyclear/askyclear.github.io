---
layout: post
title: Apache-Defference 
date: 2018-09-10 10:40:00 -0600
---
아파치 2.2.x 와 2.4.x의 차이

![_config.yml]({{ site.baseurl }}/images/config.png)


# apache-2.2.x 와 apache-2.4.x 의 차이

### NameVirtualHost 지시자
2.2.x 에서 사용하던 NameVirtualHost를 명시적으로 사용했던 것이 2.4.x 에서는 경고를 내는 것 외에 아무런 영향을 미치지 않음

### SSL Protocol
2.2.x 에서 SSLProtocol로 SSLv2 를 사용할 수 있었던 것이 2.4.x 에서는 더이상 SSLv2를 지원하지 않음

### SSLSessionCache
SSL 캐쉬를 사용한다면 2.4.x 버전에서는 mod_socache_shmcb를 로드 해야함 안할경우 아래와 같은 conf 에러가 발생
```
SSLSessionCache: 'shmcb' session cache not supported (known names: ). Maybe you need to load the appropriate socache module (mod_socache_shmcb?)
```
### LoadModule
2.2.x 에서 사용하던 mod_authz_XXXX.so 는 더이상 
### DefaultType
2.4.x 에서 이 지시어는 경고를 내는 것 외에 아무런 영향을 미치지 않음
### 권한 부여
2.2.x에서 엑세스를 제어 하던 Order, Allow, Deny는 2.4.x 에서는 새 모듈을 사용하여 엑세스를 제어함. 이전 구성과의 호환성을 위해 mod_access_compat 모듈을 제공 하지만 2.4.x에 맞춰 대체하는 것을 권함
2.2

```
 <Directory />
    Options FollowSymLinks
    AllowOverride None
    Order deny,allow
    Deny from all
</Directory>
<Directory "/home1/irteam/deploy/clean-campaign/doc_base">
    Options Indexes FollowSymLinks
    AllowOverride None
    Order allow,deny
    Allow from all
</Directory>
```

2.4

```
<Directory />
    Options FollowSymLinks
    AllowOverride none
    Require all denied
</Directory>
<Directory "/home1/irteam/deploy/clean-campaign/doc_base">
    Options Indexes FollowSymLinks
    AllowOverride None
    Require all granted
</Directory>
```

### 기타 변경
 * AcceptMutex, LockFile, RewriteLock, SSLMutex, SSLStaplingMutex 및 WatchdogMutexPath 지시문 이 하나의 Mutex 지시문 으로 대체되었음.

 * 시작 오류 
     Invalid command 'User', perhaps misspelled or defined by a module not included in the server configuration
     해결 방안
     ​	LoadModule mod_unixd
