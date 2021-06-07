---
layout: article
title: 웹 프론트 속도 개선을 위한 필수도구 소개
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
hists: https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fvelog.io%2F%40dochis%2F%25EC%259B%25B9-%25ED%2594%2584%25EB%25A1%25A0%25ED%258A%25B8-%25EC%2586%258D%25EB%258F%2584-%25EA%25B0%259C%25EC%2584%25A0%25EC%259D%2584-%25EC%259C%2584%25ED%2595%259C-%25ED%2595%2584%25EC%2588%2598%25EB%258F%2584%25EA%25B5%25AC-%25EC%2586%258C%25EA%25B0%259C&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false
---


# 😒 왜 우리 사이트는 느린가
사이트가 느려지는 원인은 다음과 같이 매우 다양합니다.
(참고로 필자는 back-end개발자입니다, 분란의 오해 없으시길 바랍니다.)

### Case.1 "일단 프론트는 나가있어" 유형
> 일단 프론트 영역은 아니라고 생각되는 유형
- 서버 / back-end / Middleware가 잘못했네.!
- 서버회사 인터넷 회선이 느리네!

### Case.2 "궁예 : 누구인가??..." 유형
> 궁예 : 누구인가? 누가 속도 이슈를 내었서?...
- 정적콘텐츠(이미지/js/css 등)의 전송속도 
- 너무 무거운 스크립트
- 너무 무거운 이미지
- 뜻하지 않은 버그로 인한 무한루프
- ~~등등 2,147,483,644가지 경우~~

* * *

> ** Tip. 느림의 기준? **
필자의 경우 2.7초를 초과 할 경우 느리다고 생각하고 있습니다.
많은 보고서에서 로딩이 3초를 초과할 경우
50% 정도의 사용자는 사이트를 이탈한다고 말합니다.!
<a href="https://bit.ly/32o2pfe" target="_blank"><br>그 보고서 자세히 보고싶어요! (akamai 보고서)</a>

# 😶 오늘 사용할 도구
오늘 소개할 도구는 JENNIFERSOFT사의 JENNIFER Front입니다.
현재 구글 계정만 있다면 **무료**로 사용하실 수 있습니다.

<a href="https://bit.ly/2TLUiaG" title="JENNIFER Front 바로가기" alt="JENNIFER Front 바로가기" taget="_blank">JENNIFER Front 바로가기</a>

> JENNIFERSOFT사 소속의 Irene님께서
'유료 계획은 현재까지는 없습니다~' 라고 답변해주셨습니다.

![JENNIFER Front 대시보드](https://images.velog.io/images/dochis/post/4e1bba42-3e74-48b0-b058-0f0e6af08869/dashboard.gif)


# 😏 와 저 그래프 이쁘다
![JENNIFER Front X-View](https://images.velog.io/images/dochis/post/405c6a0b-314f-4e8c-b093-6b504a7e5fcb/image.gif)

~~손님! 안목이 있으시군요!~~. JENNIFER의 핵심인 x-view 화면입니다.
그런데 세로축이 페이지 로딩에 소요된 시간입니다.
위쪽을 보시면 5초 이상의 노란색~빨간색 표시가 보이시죠??

그 의미는 사이트가 아주!! 느린다는 겁니다
다시 그래프를 보시면, 이쁘시지마는 않을 거예요.

JENNIFER에서는 5초 미만의 페이지의 경우 초록색, 5초~10초 구간은 노란색, 10초 이상은 빨간색으로 표시하고 있습니다.


# 🙄 왜 느릴까?

x-view 화면에서 원하시는 영역을 드래그 하시면 페이지가 느린지 알 수 있습니다.

저는 빨간색 영역을 드래그 해보겠습니다.
![x-view 드레그 방법](https://images.velog.io/images/dochis/post/b000862c-556c-4648-a8b5-53cc3d7396b3/x-view.gif)![x-view 상세보기 화면](https://images.velog.io/images/dochis/post/749e3e43-c357-4ca9-9d6f-f5a26cafdff3/image.png)

JENNIFER Front에서 사용하는 추적옵션은 다음과 같은 6종류입니다.
- 대기
	http request 시작 후 Server에서 데이터를 보내기까지 대기시간
- Server
	Server에서 데이터를 보내는 시간
- Network
	http request 시작 후 response를 받는 시점까지
- Dom
	response에서 Dom을 로드 하는 시간
- Render
	Dom을 로드하고 css/js  등을 통해 페이지를 렌더링하는 시간
- 로드
	렌더링 이후 로드에 소요된 시간
    
    
## 😣 아.. 뭐야! 둘 다 잘못했네!

맞습니다.! 위 화면에서 Network 구간에서 2.3초나 소요되었고 Dom을 로드하고 Render 하는 구간에서 3.7초나 소요되었습니다. 나머지 서버 로딩 지연 등을 포함해 사용자는 웹 페이지를 로딩하는데 9.3초를 소비하게 되었죠 (😱끔찍하네요)

🤵대표님 : 개발팀 집합....


## 🤩 어떻게 분석하나요?
가장 많이 발생되는 지연구간별 원인은 다음과 같습니다.

### 1. Server/Network 구간
- 이 두 구간은 브라우저가 html을 받기까지의 구간입니다. Network 시간
- Server 시간을 한다면 순수 전송에 소요된 시간을 알 수 있습니다. 순수 전송에 소요 된 시간이 적다면 Server의 문제로 보입니다. html를 반환하는 과정에서 오래 소요되는 구간을 찾을 필요가 있습니다.
- 순수 전송 소요시간이 길다면, Network 구성에 문제가 있을 수 있습니다. CDN 서비스를 사용하거나 캐싱 처리해 최적화 할 필요가 있습니다.

### 2. Dom/Render 구간
- 이 두 구간은 브라우저가 html을 받고 Dom을 처리하거나 Rendering 하는 과정의 이슈 구간입니다.
- Dom 구간이 느리다면 Dom이 너무 중첩되거나 무겁지 않은지 확인이 필요합니다.
- Render 구간이 느리다면 Font/Javasciprt/Css등 Rendering 지연 파일들의 Network 전송 이슈는 없었는지 확인합니다. 있었다면 Network 구간의 정비가 필요합니다. 없었다면 원인이 Javasciprt로 인한 것인지 다른 기타요인으로 인한 것인지 확인이 필요합니다.
- 타임라인을 내려보면, 파일별로 Rendering을 볼 수 있어, 개별적으로 확인 후 분석이 필요합니다.


## 😓 어렵죠..? 예시로 설명함돠!

### 🙋‍ CDN을 쓰는데 Network 구간이 느려요 

정적콘텐츠(Javascript/이미지/CSS/Font 등)의 전송을 빠르게 하고자 CDN(Contents Delivery Network)를 많이 활용하실 텐데요, CDN을 너무 맹신하시면 안 됩니다

우리는 CDN을 쓰니까 모든 지역에서 빠르겠지?! 는 오해일 수 있습니다.
지역에 따라 전송이 느린 서비스가 종종 있기 때문이죠!

JENNIFER Front의 Resource 분석에서는, 각 정적콘텐츠가 지역별로 로딩에 어느 정도 소요되었는지 볼 수 있습니다.

![Resource 분석 페이지](https://images.velog.io/images/dochis/post/b40ae23e-8f2a-4e7d-9cb5-6dd391c2b830/image.gif)

위처럼 CDN을 사용하더라도 일부 구간에서 로딩이 느릴 수 있습니다.
🏃‍♀️앗! 팀장님~~!! 우리 CDN 서비스 바꿔요!!!


### 🙋‍♂️ SPA 서비스에서 Ajax로 통신을 하고 있어요

SPA(Single Page Application)를 사용하는 경우, html과 Javascript를 먼저 반환하고 데이터는 Ajax를 통해 불러오는 경우가 많은데요!

Ajax에서 데이터 받는 시간이 길어져 사용자의 입장에서 렌더링이 느리게 느껴질 수 있답니다.!

아래와 같이 Ajax 통신만 담당하는 담당 일찐 페이지가 따로 있어요!
시간에 따른 호출수/지연시간/Error 발생 비율 등을 볼 수 있습니다.

![Ajax 페이지 이미지](https://images.velog.io/images/dochis/post/82bef7dc-a02e-43d2-a855-68769447f1a4/image.png)


### 🙋‍ JS 라이브러리는 외부에서 당겨쓰니까 빠르겠죠?
안일한 생각은 NO !
아래 사진은 '다음' CDN에서 배포하는 이미지 파일에 대한 로그입니다.

서울특별시에서 평균 4.9초의 응답속도를 보이는 것으로 확인되는데요,
이런 듯 공신력 있는 CDN 서비스에서 배포하는 파일이라도 방심은 할 수 없습니다!

서비스 로딩에 지연을 주는 중요한 파일이라면 꼭! 안정적인 CDN서비스를 통해 배포해야 합니다!

![cdn 분석](https://images.velog.io/images/dochis/post/16408823-f9b9-4438-93c0-6648739a90f5/image.png)


### 🙋‍♂️ 우리는 이미지용 서버를 따로 쓰고 있어요!
혹시 해당 서버에서 배포하는 정적 파일들은 안전하게 호스팅 되고 있을까요?!

JENNIFER Front에서는 아래 사진과 같이 정적 컨텐츠들을 url 규칙에 맞게 자동으로 그룹화하여 url 그룹 별 통계를 제공합니다.

아래 사진을 보면, 호출이 몰리는 일부 구간에서 다운로드 시간이 평균 0.27초에서 0.54초 속도가 튀는게 보이시죠?!

0.54초가 짧게 보일 수 있지만, 게시물을 예로 들면 한 게시물 내에 수십개의 이미지가 첨부될 수 있기 때문에 짧지만은 않은 지연시간입니다.!

> 😱 이미지 서버뿐 아니에요!
우리가 자주 사용하는 파일용서버/스트리밍용 서버 등에서도
비슷한 경우가 자주 발생한답니다!
각 url 그룹별로 지연원인은 없는지 자주 분석하는게 좋아요!

![리소스 그룹](https://images.velog.io/images/dochis/post/3c9236c0-052d-41b3-a784-9ddd917b885c/image.png)


### 🙋‍ 당신의 웹 폰트는 안녕하신가요?

여러분들께서 사용하시는 웹 폰트는 안녕하신가요?
국내에서 많이 사용하는 NotoSansKR, 나눔고딕 등의 폰트는 사실 매우 무겁습니다.

로딩하고 랜더링하는데에 2초 가량이 소요되죠!

때문에 웹 폰트를 항상 경량화 하고 랜더링을 비동기로 처리하는 것이 중요합니다.!

![웹 폰트 분석](https://images.velog.io/images/dochis/post/02bd846e-1413-48cf-a62d-325b0f6b2381/image.png)

웹폰트 최적화와 경량화에 대해 자세히 소개한 게시물을 알려드릴게요!
- <a href="https://bit.ly/3zakfkv" title="웹 폰트 사용과 최적화의 최근 동향 (Naver D2)" alt="웹 폰트 사용과 최적화의 최근 동향 (Naver D2)" target="_blank">웹 폰트 사용과 최적화의 최근 동향 (Naver D2)</a>
- <a href="https://bit.ly/3co3KqZ" title="웹폰트 경량화" alt="웹폰트 경량화" target="_blank">웹 폰트 경량화 (44bits)</a>
- <a href="https://bit.ly/3ghTUbh" title="웹폰트 올바른 방법으로 로딩하자" alt="웹폰트 올바른 방법으로 로딩하자" target="_blank">웹폰트 올바른 방법으로 로딩하자 (웹아틀리에)</a>



# 😳 뜻밖의 꿀팁!

Javascript Error 분석기능도 있습니다.!
Javascript Error가 발생할 경우 수집하여 브라우저 정보 등과 함께 제공합니다.!

> 😥 왜, 한번씩 그런경우 있잖아요!
특정 사용자한테만 나타나는 스크립트 에러말이에요! 
이러한 경우에도 console 상에 에러가 발생되면 자동으로 수집된답니다.!

![JS Error 분석](https://images.velog.io/images/dochis/post/054b17b5-9d7e-43eb-9949-02d3675633d3/image.png)

그 이외에도 브라우저 통계, 사용자별 통계, 리소스 그룹별 통계 등 다양한 기능이 있습니다.

> 사용자별 통계 기능은 해당 사용자가 방문했었던 모든 페이지를 시간순으로 볼 수 있어요! 디버깅에 활용 할 수 있는 뜻밖의 꿀기능이랍니다.!


# 👍 속도개선 == 원인제거
지금까지 웹 속도를 느리게 하는 다양한 원인을 분석하는 방법을 알아보았습니다.

웹 서비스의 속도를 개선하는 방법은 이러한 속도를 느리게 하는 문제를 해결하는 데에 있습니다. 지금까지 소개한 내용을 요약해보도록 하겠습니다.

**< 속도를 개선하는 방법 >**
- 서버에서 브라우저의 Request에 더 빠른 반응을 하도록 하고 더 빠른 Response를 반환하도록 개선 (캐싱 등)
- 서버에서 반환하는 Response를 빠르게 브라우저에 전달할 수 있도록 Network 구간을 개선 (gzip 등)
- 웹 페이지의 랜더링을 지연시키는 요소를 제거 (Javascript 튜닝 등)
- 정적 콘텐츠들을 더 빠르고 안정적으로 호스팅할 수 있는 CDN 서비스 선택
- 정적 콘텐츠들을 브라우저 스토리지에 캐싱
- 정적 콘텐츠 요청시 HTTP/2, HTTP/3 등의 상위 프로토콜 사용추천

으로 정리해볼 수 있을 것 같습니다.


# 👋 마치며
오늘은 APM(Application Performance Management)도구 중 front-end 개발자가 사용하면 좋을 도구에 대해 소개해보았습니다.

** 이 멋진 도구는 Javascript 몇 줄을 추가하는 것만으로 쉽게 사용하실 수 있습니다.! **

이렇게 멋진 소프트웨어를 무료로 사용할 수 있다는 점이 정말 좋은 것 같습니다.

> ps) 😎 이 게시물을 회사 대표님이 못 보게 하세요.! 

> 궁금하신 내용이나 프로그램 사용 중 막히는 부분이 있다면 언제든지 댓글로 문의하여 주세요.! 댓글은 환영입니다.!

도움을 주신 많은 분들께 감사드립니다.


# 💪 함께 하면 좋을 링크
- [무료로 서비스를 모니터링하는 3가지 방법](https://bit.ly/2ZsPcQn)
- <a href="https://bit.ly/2TLUiaG" title="제니퍼 프론트 사이트" alt="제니퍼 프론트 사이트" taget="_blank">제니퍼 프론트 사이트</a>
- <a href="https://bit.ly/2RtuOhl" title="제니퍼소프트 사이트" alt="제니퍼소프트 사이트" taget="_blank">제니퍼소프트 사이트</a>
