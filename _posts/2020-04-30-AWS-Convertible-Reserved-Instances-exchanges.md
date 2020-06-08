---
layout: article
title: AWS 컨버터블 예약인스턴스 구매 시 참고하면 좋을 점
tags: AWS
article_header:
  type: overlay
  theme: dark
  background_color: '#203028'
  background_image:
    gradient: 'linear-gradient(135deg, rgba(34, 139, 87 , .4), rgba(139, 34, 139, .4))'
    src: /assets/attachments/2020-04-30-AWS-Convertible-Reserved-Instances-exchanges/cover.png
comments: true
---


이 게시물은 AWS의 공식문서 [Exchanging Convertible Reserved Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ri-convertible-exchange.html#riconvertible-exchange-cost){:target="_blank"}의 내용과, 작성자의 경험을 합쳐 작성되었습니다.


# 예약인스턴스 = Reserved Instances (RI)
AWS의 RI의 유형은 다음과 같이 3가지로 나누어 집니다.
- 표준 RI: 가장 큰 할인 혜택(온디맨드 대비 최대 72%)을 제공하며 사용량이 꾸준한 경우에 가장 적합합니다.
- 컨버터블 RI: 할인 혜택(온디맨드 대비 최대 54%)을 제공하며 RI의 속성을 변경할 수 있습니다(교체 후 예약 인스턴스 금액이 교체 전보다 크거나 같은 경우에 한함). 표준 RI와 마찬가지로 컨버터블 RI도 사용량이 꾸준한 경우에 가장 적합합니다.
- 예정된 RI: 예약한 시간 범위 내에서 인스턴스를 시작할 수 있습니다. 이 옵션을 사용하면 용량 예약을 예측 가능하고 반복적이며 하루, 한 주 또는 한 달의 일부 동안만 필요한 일정에 맞출 수 있습니다.
RI의 종류는 추후 다른 인스턴스와 교환이 가능한 타입과 불가능한 타입 등 굉장히 많은 종류가 있습니다.



# 컨버터블 RI의 문제점
AWS의 많은 사용자분들께서 인스턴스의 타입을 추후 변경하는것을 고려하여 할인폭이 낮더라도 컨버터블을 많이 선택하실 텐데요
저 또한 이러한 고민으로 컨버터블을 선택하였습니다.

하지만 이 변경에는 다음과 같은 문제가 있습니다.
![oneCloud-Logo](/assets/attachments/2020-04-30-AWS-Convertible-Reserved-Instances-exchanges/capture_1.jpg)
> 어? 인스턴스를 교환하려는데 오류가 뜨잖아?!

컨버터블 RI 페이지에는 이런 문구가 있습니다. : "교체 후 예약 인스턴스 금액이 교체 전보다 크거나 같은 경우에 한함"

이번 게시물에서는 컨버터블 RI 구매 시 참고하면 좋을점들을 요약해보았습니다.



# RI의 교환 가능 조건
RI는 무조건 원하는 조건의 인스턴스로 변경을 할 수 있지 않습니다.
변경이 가능한 조건이 몇가지 있으며, 해당 조건이 맞지 않는 인스턴스로 변경을 시도한다면 다음 오류를 보실 수 있습니다.
> " The target configuration value is less than the input "
> 번역 : 대상 구성 값이 입력값보다 작음

## RI 교환 전 선행조건
RI 변경시 다음 조건을 만족해야합니다.
- RI의 유형이 컨버터블 RI일 것.
- 상태가 반드시 '활성' 상태일 것.
- 이전에 제출한 교환 요청이 보류중이지 않을 것.


## RI 교환규칙 (요약)
아래 설명할 RI의 교환 규칙은 다소 복잡하고 어렵기에 먼저 선 요약을 해드립니다.
- RI의 총 가치(선불금액 + 시간당 가격 * 잔여시간)보다 작은 RI로는 변경 불가
- 다른 유형, 범위로 변경 불가
- 생각보다 규칙이 까다롭다.


## RI 교환규칙
- 컨버터블 RI를 다른 유형(표준 RI 또는 예정된 RI)로 변경 불가
- 구매시 선택한 범위를 변경할 수 없음 (리전 혹은 가용영역)
- 한 번에 하나 이상의 전환 가능 예약 이스턴스를 하나의 전환 가능 예약 인스턴스로 교환 할 수 있음.
- 구매한 RI의 일부의 RI를 교환하려면 두 개 이상의 RI로 수정한 다음 하나의 RI로 교환 할 수 있음.
  - 잘 사용되지는 않습니다. [자세한 내용(영문)](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ri-modifying.html){:target="_blank"}
- 교환에 필요한 총 선불금액이 0달러 미만일 경우 AWS는 자동으로 RI의 여러 인스턴스를 제공하여 0달러 이상이 되도록 합니다.
  - 선불금 300달러의 RI를 구매하고 그 하위 옵션으로 변경하였을 때 선불금이 남게되겠죠? 그렇다면 선불금을 환불해줄 수 없으니 인스턴스를 2개 이상으로 변경하여 선불금을 더 내거나 시간당 비용이 발생되도록 하겠다는 이야기입니다.
- 전환 가능 RI의 총 가치(선불금액 + 시간당 가격 * 잔여시간)가 교환된 전환 가능 RI의 총 금액보다 작은 경우 AWS는 자동으로 전환 가능 예약 인스턴스 수량을 제공합니다. 전체 값이 교환 된 전환 가능한 RI의 값과 같거나 높아야 합니다.
  - 내가 가진 RI의 총 가치보다 작은 RI로 변경하게 될 경우 위 경우와 마찬가지로 환불해줄 수 없으니, 같은 가치의 RI로 교환하거나 더 비싼 RI로 교환하라는 말입니다.
- 더 나은 가격의 혜택을 받기위해 '선결제 없음 RI'를 '전체 선결제 RI' 혹은 '부분 선결제 RI'로 교환 할 수 있습니다.
- '전체 선결제 RI' 및 '부분 선결제 RI'를 '선결제 없음 RI'로 교환할 수 없습니다.
- '선결제 없음 RI' 교환 시 교환하고자 하는 RI의 시간당 요금이 기존 RI의 요금과 같거나 높은 경우에만 다른 '선결제 없음 RI'로 교환할 수 있습니다.
  - 변경하고자 하는 RI의 총 가치(시간당 가격 * 잔여 시간 수)가 교환하고자 하는 RI보다 작은 경우 위 경우처럼 AWS는 자동으로 인스턴스의 수량을 올려 가치가 같거나 높게 맞춥니다.
- RI를 교환 후 기존 RI는 폐기됩니다. 종료 날짜는 새 RI의 시작날짜이며, 새 RI의 종료날짜는 원래 RI의 종료날짜와 같습니다.
- 여러개의 RI를 교환하려는 경우 새 RI의 만료날짜는 미래에서 가장 먼 날짜입니다.


# 마치며
RI 변경시 준수해야 하는 규칙이 매우 많기 때문에 RI 변경은 쉽지만은 않은 작업일 것 같습니다.
모두 유의사항을 잘 숙지하시고 후회없는 RI 구매 하시길 바랍니다.!