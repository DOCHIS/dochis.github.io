---
layout: article
title: 무료로 서비스를 모니터링하는 3가지 방법
description: "서비스에 장애가 발생되는 원인은 참 다양합니다. 때문에 우리는 장애를 대비하고자 다양한 모니터링 도구를 사용합니다."
image: "/assets/attachments/2020-09-06-Ways-to-Monitor-Service-for-Free/og-bg.png"
tags: 서버 모니터링 스타트업
article_header:
  type: overlay
  theme: dark
  background_color: '#203028'
  background_image:
    gradient: 'linear-gradient(135deg, rgba(34, 139, 87 , .4), rgba(139, 34, 139, .4))'
    src: /assets/attachments/2020-09-06-Ways-to-Monitor-Service-for-Free/cover.png
comments: true
---


# 개요
서비스에 장애가 발생되는 원인은 참 다양합니다. 때문에 우리는 장애를 대비하고자 다양한 모니터링 도구를 사용합니다.
오늘은 무료로 사용가능한 도구들을 조합하여 최대한 자세히 서비스를 모니터링하는 방법을 알아보겠습니다.


* * *


# 모니터링 대상
본 게시물은 아래 대상을 무료로 모니터링 할 수 있는 방법을 소개합니다.
　  
- SMS (Server Monitoring System)
    - CPU와 메모리 사용현황
    - 프로세스별 자원점유 정보
    - 디스크와 네트워크에 대한 IO 경보
    - 디스크 사용률에 따른 경보  
　  
- Heal Cheker
    - HTTP Status 코드
    - 페이지 로딩 지연 발생유무  
　  
- APM-Front (Application Performance Management)
    - 프론트엔드 어플리이션 모니터링
    - 웹 페이지 로딩 지연 원인 분석
    - 이미지/javascript/css/front 등의 정적 리소스 로딩 지연요소 추적
    - 각 정적리소스에 대한 호출통계/지역별 로딩 지연원인 등 분석
    - javascript 오류 발생 이벤트 분석
    

* * *


# SMS 모니터링
## WhaTap Server
![WhaTap Server 대시보드 화면](/assets/attachments/2020-09-06-Ways-to-Monitor-Service-for-Free/WhaTap Server.png)
와탭은 국내 개발사에서 개발중인 서비스입니다.  
와탭은 최대 서버 5대까지 무료로 서버를 모니터링 할 수 있습니다.  
　  
**< 주요기능 >** 
- Debian / Ubuntu / Centos / Amazon Linux / Windows / SUSE 환경 지원
- 서버자원을 모니터링 할 수 있는 통합 데시보드 제공
- 서버자원 사용량이 설정범위 초과시 Slack/Telegram/어플 등으로 자동알림
- 서버자원 사용에 대한 다양한 보고서 제공 및 메일로 자동발송 기능
- 서버내 각종 이벤트 정보 수집 및 알림  

**< 장점 >** 
- 클라우드형 서비스로, 서버에 에이전트만 설치하면되서 설치가 간편
- 알림기능이 잘되어있고 플러그인 없이도 왠만한 기능이 제공
- 깔끔한 인터페이스 및 한글화  

**< 단점 >**
- 클라우드형 서비스이기 때문에  플러그인 적용이 불가하고 커스텀도 불가
- 다른 모니터링 도구에 비해 모니터링이 대상이 많지 않음



# Heal Cheker
## WhaTap URL
![WhaTap URL 대시보드 화면](/assets/attachments/2020-09-06-Ways-to-Monitor-Service-for-Free/WhaTap URL.png)
와탭 URL은 최대 10개의 URL까지 무료로 모니터링 할 수 있습니다.  
위에서 소개한 SMS 제품과 함께 무료로 제공해 하나의 계정으로 관리할 수 있습니다.  
　  
**< 주요기능 >** 
- URL을 1분 간격으로 호출하여 HTTP Status, 지연시간을 수집&모니터링
- 설정 된 내용에 따라 경보 및 이벤트 생성
- 경보 발생 시 Slack/Telegram/어플 등으로 자동알림
- 도쿄/서울/싱가포르/뭄바이/켈리포니아/프랑크푸르트 리전 지원  

**< 장점 >** 
- 클라우드형 서비스로, 설치가 필요없고 사이트에서 클릭만으로 설정 가능
- 깔끔한 인터페이스 및 한글화
- 다양한 리전을 지원하여 해외에서 접속 속도에 지연이 없는지 확인 간편  

**< 단점 >**
- 클라우드형 서비스이기 때문에  플러그인 적용이 불가하고 커스텀도 불가
- 개발된지 얼마 되지 않은 초기 서비스라 네트워크 모니터링/Socket 모니터링과 같은 기능들이 부족함
- 검사간격이 1분으로 고정되어있어 있음
- 다른 모니터링 도구에 비해 모니터링이 대상이 많지 않음



# APM-Front 모니터링
## Jennifer Front (APM-Front)
![Jennifer Front 대시보드 화면](/assets/attachments/2020-09-06-Ways-to-Monitor-Service-for-Free/Jennifer Front.png)
제니퍼 소프트는 APM 시장에서 굉장히 유명한 업체입니다.  
이미 JAVA, PHP, .NET APM 도구를 오래전부터 개발하여 그 명성이 자자합니다.  
　  
제니퍼에서 개발한 많은 APM 도구는 코어당 수백만원의 라이센스를 구매해야 사용할 수 있는 유료 소프트웨어입니다.  
　  
하지만 제니퍼에서 가장 최근에 개발한 Front-End 모니터링 도구는 현재 구글계정만 있다면 무료로 사용할 수 있습니다. (향후 유료로 전환될 수 있음)  
　  
**< 주요기능 >** 
- 페이지의 로딩 지연 분석을 위한 Server/Network/Dom Load/Render 구간별 시간 기록
- javascript 오류 로깅 및 trace 제공
- Ajax 응답시간 분석
- Browser별 페이지 로딩 시간 분석
- 호스트별 리소스 분석
- geo 위치에 따른 로딩 시간 분석
- 응답시간 분포통계/동시접속자수 와 같은 지표 제공  

**< 장점 >** 
- 깔끔하고 직관적인 인터페이스
- 자바스크립트 코드 추가만으로 간편하게 모니터링 가능
- Server/Network/Dom Load/Render 구간별 시간을 측정해 로딩 지연의 원인을 직관적으로 파악 가능
- 페이지 로딩 지연 원인 분석을 위한 다양한 기능 제공  

**< 단점 >**
- 아이피를 기준으로 geo 위치를 추적하기 때문에 위치가 다소 부정확함
- javascript 이벤트 과다발생 또는 페이지 로딩 지연 급증등의 알림기능 부재



# 기타 참고할만한 도구들
## Google Search Console
![Google Search Console 대시보드 화면](/assets/attachments/2020-09-06-Ways-to-Monitor-Service-for-Free/Google Search Console.png)
Google Search Console은 구글에서 제공하는  SEO 진단도구입니다.  
이 도구는 아이러니하게도 다음 목적을 위해 사용할 수 있습니다.

- 사이트 내 보안진단 : 구글에서 페이지를 크롤링 중 보안문제가 있는경우 페이지를 통해 알려줌
- URL HEAL CHECK : 특정 페이지에 오류가 나서 크롤러가 접근하지 못한 경우 페이지에 표시됨


* * *


# 맺으며
서비스 모니터링 중 2가지를 소개 못해드린것이 있습니다.  
바로 Aplication 레벨을 모니터링 할 수 있는 APM 도구와 Database 모니터링 도구입니다.  
　  
Back-End APM 도구는 Aplication의 언어에 따라 툴의 종류가 너무 천차 만별일 뿐 아니라 모니터링을 위한 서버 구축 등 많은 비용이 소요되고.  
Database 모니터링 도구는 왠만큼 쓸만한 도구가 모두 유료이기 때문입니다.  
　  
무료도구가 정말 많이 개발되어서 무료로 위와 같은 많은 기능을 사용할 수 있게 되었지만,  
무료로는 모든것을 해결 할 수 없다는점 꼭 참고 부탁드립니다.  
　  
ps) 제가 제니퍼소프트 직원은 아니지만 제니퍼 APM은 진짜 좋은 유료 도구입니다.



* * *


# 참고링크
- [와탭 사이트 주소](https://www.whatap.io/ko/){:target="_blank"}
- [제니퍼 APM 소개페이지](https://jennifersoft.com/ko/product/){:target="_blank"}
- [Jennifer Front 사이트](https://front.jennifersoft.com/){:target="_blank"}
- [Google Search Console 사이트](https://search.google.com/search-console/about){:target="_blank"}





