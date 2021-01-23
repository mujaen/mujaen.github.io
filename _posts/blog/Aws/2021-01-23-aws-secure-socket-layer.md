---
layout: post
title: "Load Balancer로 SSL 보안 인증서(Https) 추가하기(2)"
description: "AWS 로드밸런서에서 제공하는 보안인증서 설정하기"
category: blog
tags: aws
---

<!--more-->

* this unordered seed list will be replaced by the toc
{:toc}

## Load Balancer

들어가기에 앞서, AWS는 사용 편의성과 성능 개선을 위해 주기적으로 화면을 업데이트 하므로    
가장 최신의 포스팅을 찾아 참고하시는 것을 추천합니다. 이 포스팅은 2021년 1월 23일 기준 작성되었습니다.  
[지난 포스팅](https://mujaen.github.io/blog/2021/01/21/aws-elastic-beanstalk.html){: target="_blank"}에서 Elastic Beanstalk 생성할 때 로드밸런서를 이용하기 위해서 반드시 추가 옵션 구성으로    
로드밸런서를 등록해야 한다고 했었죠 이번 포스팅에서는 Application Load Balancer로 보안 인증서를 설정해 볼 겁니다       

![Menu](/assets/img/2021-01-23/menu.png)

> 로드밸런서 설정은 전체메뉴의 컴퓨팅 - EC2 - 로드 밸런싱 - 로드밸런서에서 설정할 수 있습니다.

### Application Load Balancer

먼저, 로드밸런서 생성을 해보죠       
로드밸런서 설정 페이지에서 좌측 상단 'Load Balancer 생성' 버튼을 눌러줍니다 

1. Application Load Balancer
1. Network Load Balancer
1. Classic Load Balancer

위의 옵션 중 Application Load Balancer을 선택해 줍니다      
Application Load Balancer는 HTTP 및 HTTPS 트래픽을 사용하는 웹 애플리케이션을 위한 기능을 제공합니다  

### Listener

기본 구성에서 이름을 입력한 뒤 리스너를 생성해 줘야 하는데요 '리스너 추가' 버튼을 눌러 아래와 같이 추가 합니다

![Listener](/assets/img/2021-01-23/listener.png)

> 리스너는 구성한 프로토콜 및 포트를 사용하여 연결 요청을 확인하는 프로세스로   
> 443 포트에 SSL 인증서와 보안 정책을 설정하게 됩니다 

### 보안 설정 구성 
가용 영역(ap-northeast-2a~2d)을 지정했다면 페이지 하단의 다음을 눌러 보안 설정을 하도록 하죠  
  




