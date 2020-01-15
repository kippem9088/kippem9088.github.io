---
title: "마이티 프로젝트 브레인스토밍"
excerpt: "마이티를 구현해보자 프로젝트의 간단한 계획을 세워봅시다."

categories:
    - Project
tags:
    - Mighty
    - Document
last_modified_at: 
---

# 마이티란 무엇인가?
[위키백과 - 마이티](https://ko.wikipedia.org/wiki/%EB%A7%88%EC%9D%B4%ED%8B%B0_(%EC%B9%B4%EB%93%9C_%EA%B2%8C%EC%9E%84))


룰은 여러 종류가 있다고는 하는데, 기본적으로는 그 시절 세종관 2308호에서 돌던 룰을 기반으로 한다. 5인 마이티이며, 사람 없으면 유령 한 명을 끼는 고스트 마이티이다. 


- 마이티: 스페이드 에이스 / 스페이드가 기루다일 경우, 다이아몬드 에이스
- 조커콜: 클로바 3 / 클로바가 기루다일 경우, 하트 3
- 딜 미스: 알파벳 1점, 10 0.5점, 조커 -1점으로 계산하여 10장의 점수 합계가 2점 이하일 경우
- 공약: 13점이 최저점이고, 아무도 안부를 경우 1점씩 내려가며 부를 수 있다.
- 기루다를 변경할 경우 공약에서 +2점이 붙는다.
- 기타 큰 부분은 마이티의 기본 룰을 따른다.


<br><br>

# 어떻게 구현하지?
## 1차 계획
**JAVA**를 사용하여 **Console**에 Text형식으로 출력되는 1차 버전 구현

### 왜 JAVA?
- 평소 회사에서 사용하지 않지만, 보편적으로 많이 쓰이는 언어
- 객체 지향적인 코드를 연습하는 것이 1차 구현의 주요 목적이기 때문 (객체 지향을 추구하는 언어는 많으나 내가 가진 예제들이 대부분 JAVA를 예시로 사용함)
- JUnit이라는 테스트 프레임워크를 사용해보고 싶음

## 2차 계획
좀 더 사용성 있는 플랫폼으로 포팅하는 것. 윈도우? 안드로이드? 웹?