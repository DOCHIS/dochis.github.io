---
layout: article
title: (AWS) owncloud 성능 테스트 결과 (ami2)
tags: AWS
article_header:
  type: overlay
  theme: dark
  background_color: '#203028'
  background_image:
    gradient: 'linear-gradient(135deg, rgba(34, 139, 87 , .4), rgba(139, 34, 139, .4))'
    src: /assets/attachments/2019-10-19-How-to-install-owncloud-on-ami2/cover.png
comments: true
---

이 게시물은 [(AWS) 나만의 파일 저장소 ownCloud 설치하기 ](/2019/10/19/How-to-install-owncloud-on-ami2.html){:target="_blank"}를 통해 설치한  
ownCloud의 성능을 테스트한 결과입니다.  
사용자의 환경에 따라 성능은 다르게 측정 될 수 있으니 참고용으로만 봐주시길 바랍니다.


# 측정목표
AWS에서 제공하는 t3인스턴스 타입 중 가장 작은 t3.nano인스턴스에서도 성능범퍼링 없이 파일의 업로드/다운로드가 가능한지 확인하기 위함임.


# ownCloud 구축환경
- OS : Amazon Linux 2 AMI (HVM), SSD Volume Type
- 인스턴스 타입 : t3.nano (2Core / 0.5GB Memory)
- 스토리지 용량 : 20GB (Elastic Block Store, gp2)
- nginx : 1.12.2 / php 7.2.22
- db : mariaDB 10.4.8
- ownCloud : 10.3 (Production)
- 서버측 암호화 : Enable

# 테스트 환경
- 인터넷 회선 : KT 1.0Gbps
- 인터넷 브라우저 : Google Chrome 77.0
    - 플러그인으로 인한 지연을 줄이기 위해 시크릿 모드로 테스트
- 테스트 컴퓨터 환경 : CPU I5-8400 2.8Ghz / 16GB Memory / SATA SSD

# 테스트 방법
- Chrome으로 100mb파일과 1gb을 첨부한 뒤 개발자도구에 나타난 통신 시간을 측정
- 각 3회씩 시도하고 그에 따른 평균값을 측정
![oneCloud-Logo](/assets/attachments/2019-10-20-owncloud-performance-test-on-ami2/data_sample.png)
< 측정방법 >
![oneCloud-Logo](/assets/attachments/2019-10-20-owncloud-performance-test-on-ami2/test_files.png)
< 테스트에 사용 된 파일 >



* * *



# < Test Report >
## 1. 100mb File
### 1-1. UPLOAD

| 순번        | 측정값      | 평균값        |
| ---------- | :--------- | :----------  |
| 1          | 6,760ms    | 6,676ms      |
| 2          | 6,450ms    |
| 3          | 6,820ms    |

약 14.97Mbps의 속도로 Upload 되었음.

### 1-2. DOWNLOAD

| 순번        | 측정값      | 평균값        |
| ---------- | :--------- | :----------  |
| 1          | 2,140ms    | 2,103ms      |
| 2          | 2,070ms    |
| 3          | 2,100ms    |

약 47.55Mbps의 속도로 Doload 되었음.


## 2. 1gb File
### 2-1. UPLOAD

| 순번        | 측정값(업로드 완료)      | 측정값(암호화처리 완료)  | 평균값(업로드)          | 평균값(암호화)         |
| ---------- | :---------             | :----------          | :----------           | :----------          |
| 1          | 74,020ms (74 Second)   | 26,130ms             | 68,693ms (68 Second)  | 24,383ms             |
| 2          | 65,020ms (65 Second)   | 24,010ms             |||
| 3          | 67,040ms (67 Second)   | 23,010ms             |||

Upload : 약 14.57Mbps의 속도로 Upload 되었음.
Encode : 1gb 파일을 암호화 하는데에 약 24초가 소요되었음.

### 1-2. DOWNLOAD

| 순번        | 측정값      | 평균값        |
| ---------- | :--------- | :----------  |
| 1          | 20,070ms (20 Second)    | 21,566ms    |
| 2          | 24,040ms (24 Second)    |
| 3          | 20,590ms (20 Second)    |

약 46.38Mbps의 속도로 Doload 되었음.



* * *


# 결론
- 다운로드와 업로드 속도가 크게 차이나는 부분은 KT 회선상의 문제로 보임.
- 2 Core / 0.5GB Memory 라는 적은 사양으로도 다운로드 / 업로드의 큰 지연 없이 클라우드 스토리지를 사용할 수 있음.
- 업로드 중 CPU 사용량이 약 15% 내외를 사용하였고, 다운로드와 암호화 중 CPU의 사용량이 45% 내외로 측정되었으며, 메모리는 10% 내외로 거의 사용되지 않아 **10인 이하 사용자가 사용하는 데에 충분할것으로 보임. (동시 업로드/다운로드 기준으로는 3인 이하)**
- 업로드 / 암호화 / 다운로드 중 단일 코어만 사용하는 모습이 보였으며, 이는 php-fpm의 설정에 의한 것으로 보임.
![다운로드/암호화 중 서버 CPU 모습](/assets/attachments/2019-10-20-owncloud-performance-test-on-ami2/server_download.png)
