---
layout: post
title: "Elastic Beanstalk로 애플리케이션 등록과 관리"
description: "Elastic Beanstalk 설정하기"
category: blog
tags: aws
---

<!--more-->

* this unordered seed list will be replaced by the toc
{:toc}

## Create

들어가기에 앞서, AWS는 사용 편의성과 성능 개선을 위해 주기적으로 화면을 업데이트 하므로    
가장 최신의 포스팅을 찾아 참고하시는 것을 추천합니다. 이 포스팅은 2021년 1월 21일에 작성되었습니다.   
Elastic Beanstalk는 구축한 애플리케이션 서버를 관리할 때 사용되죠 오늘은 Elastic Beanstalk의 환경을   
생성해보고 관리하는 방법을 알아볼 겁니다.  

![Menu](/assets/img/2021-01-21/menu.png)

> Elastic Beanstalk 설정은 전체메뉴의 컴퓨팅 - Elastic Beanstalk에서 설정할 수 있습니다.

### 티어 선택

Elastic Beanstalk 환경 설정을 위해서 우측의 '새 환경 설정' 버튼을 눌러 줍니다.  
환경 티어 선택에서 '웹 서버 환경'을 선택하고 '선택' 버튼을 눌러주세요

![Select Tier](/assets/img/2021-01-21/tier.png)

### 이름 입력

다음은 애플리케이션 이름을 입력해 줍니다 이름을 입력을 하게 되면 자동으로      
아래에 -env 가 붙은 환경 이름이 만들어지는 것을 확인할 수 있습니다  

![Add Name](/assets/img/2021-01-21/name.png)

> 이때, 환경 이름은 EC2에 자동으로 생성되는 인스턴스 명입니다! 

### 플랫폼 선택

Java, Ruby, Tomcat, Node.js, PHP 등 자신의 프로젝트에 해당되는 플랫폼을 선택하면 됩니다.         
저는 Tomcat으로 서버를 구동하기 때문에 Tomcat을 선택하고 진행하겠습니다

![Select Platform](/assets/img/2021-01-21/platform.png) 

### 코드 등록

따로 설정한 파일이 없으므로 샘플 애플리케이션을 사용하여 진행하겠습니다    
마지막으로 아래의 '환경 생성' 버튼을 누르면 5-10분 뒤에 애플리케이션이 만들어진 것을 확인할 수 있습니다    
환경이 생성되면 EC2의 인스턴스도 자동으로 생성됩니다! 

![Add Code](/assets/img/2021-01-21/code.png) 

> '추가 옵션 구성' 버튼을 누르면 추가적인 옵션을 선택할 수 있는데요        
> 만약 로드밸런서를 이용하여 보안 인증서를 등록하려면 반드시 이곳에서 설정해야     
> 나중에 다시 설치해야 하는 번거로움을 피할 수 있습니다

## Delete

생성한 환경을 삭제해 보도록 하죠 해당 환경의 좌측 체크박스를 누른 뒤 우측의 작업을 눌러 '환경 종료'를  
눌러주면 5-10분 뒤 환경이 종료되고 하루 정도 지나면 자동으로 리스트에서 삭제됩니다.  

![Delete](/assets/img/2021-01-21/delete.png) 

> Elastic Beanstalk 환경 설정은 전체메뉴의 컴퓨팅 - Elastic Beanstalk - 환경에서 설정할 수 있습니다.
