---
layout: article
title: 웹 프론트 속도 개선을 위한 필수도구 소개 (무료)
description: "왜 우리사이트만 느린가? 쉽게 파악하는 방법"
image: "https://media.vlpt.us/images/dochis/post/16986537-cf95-48c6-ada7-3f8857f97c45/GregariousGenuineKoalabear-size_restricted.gif"
tags: apm front-end monitoring 성능개선
article_header:
  type: overlay
  theme: dark
  background_color: '#203028'
  background_image:
    gradient: 'linear-gradient(135deg, rgba(34, 139, 87 , .4), rgba(139, 34, 139, .4))'
    src: https://media.vlpt.us/images/dochis/post/16986537-cf95-48c6-ada7-3f8857f97c45/GregariousGenuineKoalabear-size_restricted.gif"
comments: true
---

[![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fvelog.io%2F%40dochis%2F%25EC%259B%25B9-%25ED%2594%2584%25EB%25A1%25A0%25ED%258A%25B8-%25EC%2586%258D%25EB%258F%2584-%25EA%25B0%259C%25EC%2584%25A0%25EC%259D%2584-%25EC%259C%2584%25ED%2595%259C-%25ED%2595%2584%25EC%2588%2598%25EB%258F%2584%25EA%25B5%25AC-%25EC%2586%258C%25EA%25B0%259C&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)


# 😒 왜 우리사이트는 느린가
사이트가 느려지는 원인은 다음과 같이 매우 다양합니다.
(참고로 필자는 back-end개발자입니다, 분란의 오해없으시길 바랍니다.)

### Case.1 "일단 프론트는 나가있어" 유형
> 일단 프론트 영역은 아니라고 생각되는 유형
- 서버 / back-end / Middleware가 잘못했네.!
- 서버회사 인터넷 회선이 느리네!

### Case.2 "궁예 : 누구인가??..." 유형
> 궁예 : 누구인가? 누가 속도 이슈를 내었서?...
- 정적콘텐츠(이미지/js/css 등)의 전송속도 
- 너무 무거운 스크립트
- 너무 무거운 이미지
- ~~등등 2,147,483,644가지 경우~~


# 😶 오늘 사용할 도구
오늘 소개할 도구는 JENNIFERSOFT사의 JENNIFER Front입니다.
현재 구글 계정만있다면 **무료**로 사용하실 수 있습니다.

[JENNIFER Front 바로가기](https://front.jennifersoft.com/)

> JENNIFERSOFT사 소속의 Irene님께서
'유료 계획은 현재까지는 없습니다~' 라고 답변해주셨습니다.

![JENNIFER Front 대시보드](https://images.velog.io/images/dochis/post/ba4f6993-baa2-41f6-8fea-795474b09bef/image.png)


# 😏 와 저 그레프 이쁘다
![JENNIFER Front X-View](https://images.velog.io/images/dochis/post/11d5d2e3-bcfa-4a58-8dd9-b4986ea9dfc9/image.png)

네 잘 보셨습니다. JENNIFER의 핵심인 x-view 화면입니다.
그런데 세로축이 페이지 로딩에 소요 된 시간입니다.
왼쪽을 보시면 5초이상의 노란색~빨간색 표시가 보이시죠??

그 의미는 사이트가 굉장히 느리다는겁니다.
다시 그레프를 보시면, 이쁘시지만은 않을꺼에요.


# 🙄 왜 느릴까?

x-view 화면에서 원하시는 영역을 드레그 하시면 해당 웹 페이지가 왜 느리게 떳는지 알 수 있습니다.

저는 빨간색 영역을 드레그 해보겠습니다.
![드레그 방법](https://images.velog.io/images/dochis/post/1108f680-b3f5-48ea-9ece-ade4ca0228d0/image.png)

![x-view 상세보기 화면](https://images.velog.io/images/dochis/post/749e3e43-c357-4ca9-9d6f-f5a26cafdff3/image.png)

JENNIFER Front에서 사용하는 추적옵션은 다음과 같은 6종류입니다.
- 대기 :
	http request 시작 후 Server에서 데이터를 보내기 까지 대기시간
- Server :
	Server에서 데이터를 보내는 시간
- Network :
	http request 시작 후 response을 받는 시점까지
- Dom :
	response에서 Dom을 로드 하는 시간
- Render :
	Dom을 로드 하고 css/js  등을 통해 페이지를 렌더링 하는 시간
- 로드 :
	렌더링 이후 로드에 소요 된 시간
    
    
## 😣 아..;; 뭐야 둘 다 잘못했네;;

맞습니다.! 위 화면에서 Network 구간에서 2.3초나 소요되었고 Dom을 로드하고 Render 하는 구간에서 3.7초나 소요되었습니다. 나머지 서버 로딩 지연 등을 포함해 사용자는 웹 페이지를 로딩하는데 9.3초를 소비하게 되었죠 (😱끔찍하네요)


## 🤩 어떻게 분석하나요?
가장 많이 발생되는 지연구간 별 원인은 다음과 같습니다.

### < Server/Network 구간 >
- 이 두 구간은 브라우저가 html을 받기 까지의 구간입니다. Network 시간 - Server 구간을 한다면 순수 전송에 소요 된 시간을 알 수 있습니다. 순수 전송에 소요 된 시간이 적다면 Server의 문제로 보입니다. html를 반환하는 과정에서 오래 소요되는 구간을 찾을 필요가 있습니다.
- 순수 전송 소요시간이 길다면, Network 구성에 문제가 있을 수 있습니다. CDN 서비스를 통해 속도를 줄이거나 할 필요가 있습니다.

### < Dom/Render 구간 >
- 이 두 구간은 브라우저가 html을 받고 Dom을 처리하거나 Rendering 하는 과정의 이슈 구간입니다.
- Dom 구간이 느리다면 Dom이 너무 중첩되거나 무겁지 않은지 확인이 필요합니다.
- Render 구간이 느리다면 Font/Javasciprt의 Network 전송 이슈는 없었는지 확인합니다. 있었다면 Network 구간의 정비가 필요합니다. 없었다면 원인이 Javasciprt로 인한것인지 다른 기타요인으로 인한것인지 확인이 필요합니다.
- 타임라인을 내려보면, 파일별로 Rendering을 볼 수 있어, 개별적으로 확인 후 분석이 필요합니다.


### 🙋‍CDN을 쓰는데 Network가 느려요 

정적콘텐츠(Javascript/이미지/CSS/Font 등)의 전송을 빠르게 하고자 CDN(Contents Delivery Network)를 많이 활용하실 텐데요, CDN을 너무 맹신하시면 안됩니다.

우리는 CDN을 쓰니까 모든 지역에서 빠르겠지?! 는 오해일 수 있습니다.
지역에 따라 느린 전송이 서비스가 종종 있기 때문이죠!

JENNIFER Front의 Resource 분석에서는, 각 정적콘텐츠가 지역별로 로딩에 어느정도 소요되었는지 볼 수 있습니다.

![Resource 분석 페이지](https://images.velog.io/images/dochis/post/7fa296bd-1e59-4e1c-bc3b-4af6d0f03a5d/image.png)

위 처럼 CDN을 사용하더라도 일부 구간에서 로딩이 느릴 수 있습니다.
🏃‍♀️앗! 팀장님~~!! 우리 CDN 서비스 바꿔요!!!


### 🙋‍♂️ SPA 서비스에서 Ajax로 통신을 하고있어요

아래와 같이 Ajax 통신만 담당하는 담당 일찐 페이지가 따로있어요!
시간에 따른 호출수/지연시간/Error 발생 비율 등을 볼 수 있습니다.

![Ajax 페이지 이미지](https://images.velog.io/images/dochis/post/82bef7dc-a02e-43d2-a855-68769447f1a4/image.png)


### 🤷‍♂️ 또 다른기능은 없나요?

Javascript Error 분석기능도 있습니다.!
Javascript Error가 발생할 경우 수집하여 브라우저 정보 등과 함께 제공합니다.!

디버깅할 때 정말 편하겠죠!?

![JS Error 분석](https://images.velog.io/images/dochis/post/054b17b5-9d7e-43eb-9949-02d3675633d3/image.png)

그 이외에도 브라우저 통계, 사용자별 통계, 리소스 그룹별 통계 등 다양한 기능이 있습니다.



## 👋 마치다.
오늘은 APM(Application Performance Management)도구 중 front-end 개발자가 사용하면 좋을 도구에 대해 소개해보았습니다.

이렇게 멋진 소프트웨어를 무료로 사용할 수 있다는 점이 정말 좋은것 같습니다.

도움을 주신 많은 분들께 감사드립니다.


## 💪 함께 하면 좋을링크
- [무료로 서비스를 모니터링하는 3가지 방법](https://blog.dochis.net/2020/09/06/Ways-to-Monitor-Service-for-Free.html)
- [제니퍼 프론트 사이트](https://front.jennifersoft.com/)
- [제니퍼소프트 사이트](https://jennifersoft.com/)


