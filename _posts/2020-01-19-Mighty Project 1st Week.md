---
title: "마이티 프로젝트 - 1주차"
excerpt: "마이티 프로젝트 1주차 진행상황"

categories:
    - Mighty Project
tags:
    - Mighty
    - Document
last_modified_at: 
---

## 1주차 진행상황
![Project 상황](/Images/GhostProject_1st.JPG)
- Done
    - 게임의 구조를 담당할 패키지들을 생성하고, 기본적인 기능을 구현했다. (Card, Deck, Game, Player, Rule)
    - 추후에 입출력 방식이 변경될 것을 대비하여, 입출력과 관련된 기능은 IOController 클래스에 따로 작성중이다.
    - 각각의 클래스의 public 메소드들은 대부분 테스트를 작성하였다.    
    - 게임을 시작하면 자동으로 5인의 플레이어가 생성되고 각각의 플레이어에게 랜덤으로 10장의 카드가 배분된다.
- To do
    - 플레이어 5인이 돌아가면서 공약을 내걸고 주공과 친구를 결정하는데까지 구현해보자.
    - Java로 실행 가능한 파일 생성하는 법 알아보기 (jar?)
- Issues
    - 구현된 내용을 문서화해주는 툴이 있으면 좋겠다... IntelliJ의 UML Diagram 생성 기능이 있다는데 찾을 수가 없다.
    - 몇몇 메소드들에 대한 테스트 케이스가 작성되지 않았다. 메소드가 테스트하기에 적합해보이지 않는데, 차차 개선 방안을 생각해보며 일단 기능 구현에 집중해보기로 했다.    