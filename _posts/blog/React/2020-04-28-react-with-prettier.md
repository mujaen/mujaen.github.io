---
layout: post
title: "React(Next.js)로 만드는 채팅 애플리케이션(2)"
description: "Prettier(Linter) 사용하기"
category: blog
tags: react
---

<!--more-->

* this unordered seed list will be replaced by the toc
{:toc}

## Prettier

이번 포스팅에서는 가장 대중적인 코드 포맷터인 Prettier를 적용해 보도록 할 건데요   
만약 Prittier 사용을 원하지 않는다면 다음 포스팅으로 넘어가도 전체적인 흐름에 영향이 없습니다     
IDE에서 린터를 지원하지 않는다면 따로 설치가 필요합니다 터미널을 열어 아래의 명령어를 입력해 줍니다.

```shell
npm install --save-dev --save-exact prettier
```

> exact 옵션을 사용하면 버전이 바뀌면서 생기는 스타일의 변화에 대응할 수 있습니다

### .prettierignore

Prittier가 설치되면 루트 경로에 .prettierignore 파일을 생성하고 아래와 같이 포맷하지 않을 파일을 Ignore 처리합니다

```shell
# Ignore artifacts:
build
coverage
```

### 포맷

Ignore 설정을 했다면 포맷을 하기 위해 터미널을 열고 아래의 명령어를 입력해 줍니다  
'src' 경로의 파일 형식에 대해서만 포맷을 하겠습니다. 

```shell
npx prettier --write src/
```

> 'npx prettier --write .' 을 하게되면 루트 경로부터 모든형식에 지정되어 프로젝트가 커질수록 시간이 지연됩니다. 

## Options

루트 경로에 prettier.config.js 파일을 하나 만들어 주세요   
아래의 옵션을 사용하여 들여 쓰기, 공백 사용, 세미콜론, 브래킷 간격 등 포맷 시에 옵션을 지정할 수 있습니다

```javascript
module.exports = {
    tabWidth: 4,
    semi: false
};
```

### Tab Width

들여 쓰기 시 공백 수를 지정합니다

|----|----|----|
|Default|CLI Override|API Override|
|2|--tab-width &lt;int&gt;|tabWidth: &lt;int&gt;|

### Semicolons

명령문 마지막에 세미콜론을 추가합니다

|----|----|----|
|Default|CLI Override|API Override|
|true|--no-semi|semi: &lt;bool&gt;|


> 이외에 추가 옵션 사항은 [Prettier 공식문서](https://prettier.io/docs/en/options.html){: target="_blank"}를 참고해 주세요!
 



