---
layout: single
title:  "Apache2 400 Bad Request"
categories: apache2
tag: apache2
toc: true
author_profile: false
sidebar:
    nav: "docs"
search: true
---

![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https://{{ site.url | remove_first: 'https://' | remove_first: 'http://' }}{{ page.url }}&count_bg=%23FFCF00&title_bg=%230045FF&icon=macys.svg&icon_color=%23FAFF00&title=hits&edge_flat=false)

## Apache2 400 Bad Request  

[참고](https://httpd.apache.org/docs/2.4/mod/core.html#httpprotocoloptions){: .btn .btn--warning}  
[출처](https://sarc.io/index.php/httpd/2165-apache-400-error-n-whitespace){: .btn .btn--warning}  

Apache 2.4.25 이후 버전부터 보안성이 강화되었다.  
HttpProtocolOptions Strict 설정이 기본으로 적용된다.  

이 설정으로 인해, 보통 "\n" "\n\n" 등이 아닌 "\r\n"(CRLF)만 줄바꿈으로 판단하게 된다.  
또한 whitespace(공백)을 disallow 하기에, 요청 url에 공백이나 공백이 포함된 파일명 같은 것이 있다면 400이 발생할 수 있다.  
즉, 동일한 내용의 데이터에서 줄바꿈 문자가 다른 경우, 서버 응답이 달라진다.  
이것은 "HttpProtocolOptions Unsafe" 설정을 추가하여 해결할 수 있다.  

나는 apache2.conf 에서 
```
sudo nano /etc/apache2/apache2.conf
```  

```
# Include list of ports to listen on
Include ports.conf

// 위의 내용 바로 밑에 아래의 내용을 추가했다. 
HttpProtocolOptions Unsafe
```  