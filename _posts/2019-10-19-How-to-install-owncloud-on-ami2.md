---
layout: article
title: (AWS) 나만의 파일 저장소 ownCloud 설치하기
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


회사에서 파일 공유를 위해 클라우드가 필요하게 되었는데, 기존 상용 스토리지를 찾아보니 가격도 비싸고 첨부 용량에 제한이 있거나 속도가 느리다는 점 등의 문제점이 있어 고민하게 되었다.
때문에 **무료**로 사용할 수 있는 클라우드 저장소 소프트웨어를 찾아보게 되었고,
이번 글에서는 AWS에서 제공하는 **Amazon Linux 2 AMI 환경**에서 **ownCloud를 설치하는 방법**에 대해 소개해보려고 한다.
![oneCloud-snapshot](/assets/attachments/2019-10-19-How-to-install-owncloud-on-ami2/owncloud_snapshot.png)


* * *


# onwCloud의 특징
![oneCloud-Logo](/assets/attachments/2019-10-19-How-to-install-owncloud-on-ami2/owncloud_logo.png)
- 웹 기반으로 어디서든 사용 가능
- 서버측 스토리지 암호화 기능 제공
- PC 동기화 프로그램 제공 (Mac/Windows/Linux지원)
- 달력 및 주소록 (CalDAV 사용)
- Ampache를 사용한 음악 스트리밍
- OpenID나 LDAP를 사용한 사용자 및 그룹 관리
- 그룹이나 공개 URL로 파일 공유 가능
- pdf.js를 사용한 PDF 뷰어
- 스마트폰 / 태블릿 애플리케이션 사용 가능(IOS/android 지원)



# 구축환경
- OS : Amazon Linux 2 AMI (HVM), SSD Volume Type
- 인스턴스 타입 : t3.nano (2Core / 0.5GB Memory)
- 스토리지 용량 : 20GB (Elastic Block Store, gp2)
- nginx : 1.12.2 / php 7.2.22
- db : mariaDB 10.4.8
- ownCloud : 10.3 (Production)

## 구축시 예상 사용비용
- ownCloud : 무료
- 인스턴스 비용 : 약 4,600원
- 스토리지 비용 : 약 2,400원
- 통신구간 비용 : 약 3,000원 (Outbound Data Transfer 10GB 기준)
- 합계 : 월 약 1만원
- 주) 위 사양으로 구축 시 어느 정도의 퍼포먼스가 나오는지는 [성능측정결과 게시물](/2019/10/20/owncloud-performance-test-on-ami2.html){:target="_blank"} 참고.

* * *


# 설치방법

## Step. 1 : yum update
먼저 yum 패키지들을 업데이트
~~~~
sudo yum update -y
~~~~



## Step. 2 : php install
amazon-linux-extras 패키지를 사용하여 php 설치
~~~~
sudo amazon-linux-extras install php7.3 -y
~~~~

php 확장모듈 설치
~~~~
sudo yum install php-cli php-common php-gd php-mbstring  php-mysqlnd php-pdo php-fpm php-xml curl -y
sudo yum install php-opcache php-zip php-bcmath libzip-devel php-devel php-pear gcc zlib-devel php-intl -y
~~~~

php-fpm 설정변경
~~~~
sudo vi /etc/php-fpm.d/www.conf
~~~~
다음 설정을 찾아 변경
~~~~
user = nginx
group = nginx
~~~~
php.ini 설정변경
~~~~
sudo vi /etc/php.ini
~~~~
다음 설정을 찾아 변경
~~~~
date.timezone = Asia/Seoul
~~~~

(선택사항) 첨부파일 용량을 변경하려면 다음 설정도 같이 변경할 것.
- sudo vi /etc/php.ini 수정사항
    - upload_max_filesize : 파일 하나당 최대 용량 (기본:2MB)
    - post_max_size : 1회 전송 최대용량 (기본 :8MB)
    - max_execution_time : php의 최대 실행시간 (기본 :30초)
    - max_input_time : 데이터를 입력받는 최대시간 (기본 :60초)
    - memory_limit : 최대 메모리 용량 (기본 :128MB)
- sudo vi /etc/nginx/nginx.conf 추가사항
    - client_max_body_size ???M; 추가
- 수정 후 sudo systemctl restart nginx; sudo systemctl restart php-fpm; 명령어로 재시작 해야 적용됨.
- client_max_body_size = post_max_size > upload_max_filesize >= memory_limit 이 성립해야 파일업로드가 실패되지않음

설정 적용을 위한 php-fpm 재시작
~~~~
sudo systemctl restart php-fpm
sudo systemctl enable php-fpm.service
~~~~

session 디렉토리 퍼미션 변경
~~~~
sudo chmod 770 /var/lib/php/session/ -R
sudo chown nginx.nginx /var/lib/php/session/ -R
~~~~



## Step. 4 : nginx install
~~~~
sudo amazon-linux-extras install nginx1.12 -y
~~~~

부팅시 nginx가 자동시작되도록 설정
~~~~
sudo systemctl enable nginx.service
~~~~

nginx config 열기
~~~~
sudo vi /etc/nginx/nginx.conf
~~~~

80 default server 설정 중
root를 다음과 같이 수정
~~~~
    server {
        listen       80 default_server;
        listen       [::]:80 default_server;
        server_name  _;
        #root         /usr/share/nginx/html;
        root         /usr/share/nginx/html/owncloud;
        
        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;

        location / {
        }

        error_page 404 /404.html;
            location = /40x.html {
        }

        error_page 500 502 503 504 /50x.html;
            location = /50x.html {
        }
    }
~~~~

nginx restart
~~~~
sudo systemctl restart nginx.service
~~~~



## Step. 5 : onwCloud Download
~~~~
cd /usr/share/nginx/html/
sudo wget -q https://download.owncloud.org/community/owncloud-10.3.0.zip
sudo unzip owncloud-10.3.0.zip
sudo chmod 775 owncloud -R
sudo chown nginx.nginx owncloud -R
~~~~


## Step. 6 : mariaDB install
mariaDB는 remi-repo를 사용하여 설치
~~~~
sudo vi /etc/yum.repos.d/MariaDB.repo
~~~~
~~~~
[mariadb]
name = MariaDB
baseurl = http://yum.mariadb.org/10.4/centos7-amd64
gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
gpgcheck=1
~~~~
~~~~
sudo yum install MariaDB -y
~~~~

부팅시 mariaDB가 자동시작되도록 설정 및 시작
~~~~
sudo systemctl enable mariadb.service
sudo systemctl start mariadb
~~~~

아래 명령어 실행 후 원하는 비밀번호 입력
~~~~
sudo /usr/bin/mysqladmin -u root password 
New password: <비밀번호 입력>
Confirm new password: <비밀번호 확인>
~~~~

oneCloud에서 사용할 Database를 만들기 위해 mysql에 로그인 후 database 생성
~~~~
mysql -uroot -p
Enter password: <비밀번호 입력>
CREATE DATABASE owncloud default CHARACTER SET UTF8;
exit;
~~~~


## Step. 7 : ownCloud Install
브라우저를 통해 해당 서버의 아이피로 접속 후 설정을 마무리합니다.
- 관리자 계정 : 사용하고자 하는 아이디와 비밀번호를 입력합니다.
- 저장소 및 데이터베이스 :
    - 데이터베이스 설정 : MySQL / MariaDB
    - 데이터베이스 사용자 : root
    - 데이터베이스 암호 : <MySQL 설치시 설정한 암호>
    - 데이터베이스 이름 : owncloud
    - 데이터베이스 호스트 : localhost
- 위 정보 입력 후 '설치 완료'를 클릭하면 잠시 후 설치가 완료됩니다.
![owncloud_first_page](/assets/attachments/2019-10-19-How-to-install-owncloud-on-ami2/owncloud_first_page.png?c=191020)

설치완료 후 로그인화면
![owncloud_login.png](/assets/attachments/2019-10-19-How-to-install-owncloud-on-ami2/owncloud_login.png)


## Complete
![owncloud_logined.png](/assets/attachments/2019-10-19-How-to-install-owncloud-on-ami2/owncloud_logined.png)




# (중요) 맺음말 및 주의사항
현재 이 가이드만 따라 할 경우 SSL 보안인증서와 서버측 파일보호가 되지 않은 상태로 완료됩니다.  
SSL 보안 인증서가 없다면, 파일이 네트워크 통신구간에서 감청 되는 등의 보안 위험이 있을 수 있으며,
서버측 파일보호가 켜져있지 않다면, 만약 있을 수 있는 서버의 해킹 등의 공격 시 파일을 보호할 수 없습니다.

## 서버측 보호 사용
로그인 후 화면에서 오른쪽 아이디 클릭 > 설정 접속
좌측 관리자 > 암호화 > 서버 측 암호화 사용 체크
주의사항을 모두 숙지한 뒤 '암호화사용' 클릭
![owncloud_security.png](/assets/attachments/2019-10-19-How-to-install-owncloud-on-ami2/owncloud_security.png)