---
layout: post
title: "Load Balancer로 SSL 보안 인증서(Https) 추가하기(1)"
description: "AWS Certificate Manager로 인증서 요청"
category: blog
tags: aws
---

<!--more-->

* this unordered seed list will be replaced by the toc
{:toc}

## Certificate Manager

들어가기에 앞서, AWS는 사용 편의성과 성능 개선을 위해 주기적으로 화면을 업데이트 하므로    
가장 최신의 포스팅을 찾아 참고하시는 것을 추천합니다. 이 포스팅은 2021년 1월 22일에 작성되었습니다.  
AWS는 Certificate Manager에서 무료로 공인 인증서를 제공합니다 다만, 인증서를 적용할 로드밸런서는 요금이 부과됩니다  
이번 포스팅에서는 AWS의 Certificate Manager에서 무료로 보안 인증서를 만들어 볼 겁니다       

![Menu](/assets/img/2021-01-22/menu.png)

> Certificate Manager 설정은 전체메뉴의 검색기능을 통해 접근할 수 있습니다.

### 인증서 요청

인증서를 요청하기 전에 도메인은 미리 구입 하였다는 전제 하에 진행하도록 하죠     
Certificate Manager 설정 페이지에서 좌측 상단 '인증서 요청' 버튼을 눌러줍니다 

공인 인증서 : ACM에서 발급하는 Amazon의 공인 인증서입니다 도메인 검증을 하기 때문에 별도의 알람이 발생하지 않습니다.  
사설 인증서 : 도메인 검증을 하지 않기 때문에 알람이 발생하는 사설 CA 기반 인증서입니다

위의 옵션 중 '공인 인증서 요청'을 선택해 줍니다 아래 '인증서 요청' 버튼을 눌러줍니다.

### 도메인 이름 추가

도메인 이름을 추가해 주세요 

![Domain](/assets/img/2021-01-22/domain.png)

> 다른 이름을 추가하면 사용자가 등록된 여러 도메인으로 접속할 수 있습니다

### 검증

공인 인증서는 도메인 확인을 위해 DNS 검증과 이메일 검증을 진행해야 합니다  
이번 포스팅에서는 DNS 검증을 통해 인증서를 요청할 겁니다  

태그 추가는 생략하고 넘어갈 수 있습니다 도메인 이름과 검증 방법을 선택했다면 '확인 및 요청' 버튼을 눌러   
검토를 진행하게 됩니다 [지난 포스팅](https://mujaen.github.io/blog/2021/01/18/aws-route-53.html){: target="_blank"}에서 Route53에 DNS 정보를 등록해서 도메인을 연결했었죠      
검증을 위해서 아래의 CNAME 기록을 Route53에 DNS 구성에 추가하면 검증이 완료됩니다     

![Validation](/assets/img/2021-01-22/validation.png)
  




