---
layout: post
title: Apache-Defference 
date: 2018-09-14 10:50:00 -0600
---
Web Server 와 WAS


## Web Server와 WAS

#### Web Server 
웹 브라우저와 같은 클라이언트로에게 HTML 문서, 이미지와 같은 정적인 콘텐츠를 제공하는 서버.

#### WAS(Web Application Server) 
서버 단에서 애플리케이션을 동작할 수 있도록 지원하는 프로그램(컨테이너)으로 DB와의 연동등의 동적인 콘텐츠를 제공할 때 사용되는 서버이다.

#### Web Server와 WAS를 분리하여 사용하는 이유

WAS에서 정적인 콘텐츠와 동적인 콘텐츠를 두개다 제공하게 되면 두개의 처리를 동시에 하기 때문에 최적화 측면에서 그다지 바람직하지 않다. 그래서 Web Server와 WAS를 분리하여 HTML, CSS, JS파일을 Web Server에서 처리하게 하고 동적인 웹 애플리케이션 처리는 WAS에서 하게 분리하여 효과적으로 분산처리 하게 하는 것이다.

![아키테쳐](/images/web_app_architecture.png "웹어플아키테쳐")

이미지 출처 : https://cybersecuritynews.co.uk/popular-web-application-attacks-and-recommendations/
