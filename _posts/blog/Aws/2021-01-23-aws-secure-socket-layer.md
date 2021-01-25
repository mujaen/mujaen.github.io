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
가장 최신의 포스팅을 찾아 참고하시는 것을 추천합니다. 이 포스팅은 2021년 1월 23일에 작성되었습니다.  
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

가용 영역(ap-northeast-2a~2d)을 지정한 뒤 추가 서비스와 태그는 생략하고 진행하겠습니다  
이제 설정페이지 하단의 다음을 눌러 보안 설정을 하도록 하죠  

인증서 유형은 'ACM에서 인증서 선택'을 눌러주세요 정책은 원하는 보안 정책을 선택합니다 

![ACM](/assets/img/2021-01-23/acm.png)

### 보안 그룹 구성 

다음은 보안 그룹을 구성해야 하는데요 보안 그룹은 로드 밸런서에 대한 트래픽을 제어하는 방화벽 규칙 세트입니다  
[지난 포스팅](https://mujaen.github.io/blog/2021/01/21/aws-elastic-beanstalk.html){: target="_blank"}에서 Elastic Beanstalk로 애플리케이션을 만들 때 추가 옵션 구성을 통해 로드밸랜서를 설정하였다면 자동으로        
아래 '기존 보안 그룹 선택' 리스트에 ELB 보안 그룹이 생성된 걸 확인할 수 있습니다   
리스트에서 Elastic Beanstalk created security group used when no ELB security groups... 항목을 찾아 체크를 합니다

### 라우팅 구성

로드 밸런서는 사용자가 지정한 프로토콜 및 포트를 사용하여 여기에 설정한 대상으로 요청을 라우팅하고      
대상에 대한 상태 점검을 수행할 겁니다 이 단계에서 지정한 대상 그룹은 이 로드 밸런서에 구성된 모든 수신기에 적용됩니다.  
다른 부분은 기존 설정을 그대로 유지하고 이름만 변경한 뒤 다음 단계로 이동해 주세요.  

![Routing](/assets/img/2021-01-23/routing.png)

### 대상 등록

'등록된 항목에 추가' 버튼을 눌러 등록된 대상에 인스턴스를 등록합니다  

![Instance](/assets/img/2021-01-23/instance.png)


### 검토

드디어 마지막 검토 단계입니다! 검토 단계에서는 이전 단계에서 등록한 내용이 맞는지 확인하고  
이상이 없다면 우측 하단의 '생성' 버튼을 눌러 로드 밸런서를 생성합니다.

### Route53 

로드밸런서를 생성하였다고 바로 Https 연결이 되는 것은 아닙니다  
[지난 포스팅](https://mujaen.github.io/blog/2021/01/18/aws-route-53.html){: target="_blank"}에서 Route53에 등록된 레코드를 변경하는 방법을 참고해 주세요!
  
이제 사이트에 제대로 적용이 되었는지 확인해 봐야겠죠?  
브라우저를 열어 주소를 입력한 뒤 좌측의 자물쇠 아이콘을 누르면 정상적으로 보안 연결이 되어 있는 것을 확인할 수 있습니다!

![Site](/assets/img/2021-01-23/site.png)







