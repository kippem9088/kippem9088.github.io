---
title: "마이티 프로젝트 - 3주차"
excerpt: "마이티 프로젝트 3주차 진행상황"

categories:
    - Mighty Project
tags:
    - Mighty
    - Document
last_modified_at:
---


## 3주차 진행상황
![3주차 진행상황](/Images/GhostProject_3rd.JPG)
- Done
    - 공약으로 노기루도 선언할 수 있도록 수정하였다. (현재 구현 상황으로는 노기루의 경우 CardSuit가 null인 경우를 의미하는데, 구조가 명확하지 않으므로 수정이 필요할 듯 하다.)
    - 선출된 주공이 덱에 남아있는 3장의 카드를 보고 버릴 카드 3장을 선택할 수 있게 되었다.
    - 주공이 카드를 선택한 후, 기루를 변경할 지 여부를 결정할 수 있다. 이 때, 기루를 변경하면 공약 점수가 2점 올라간다.
    - 이 후 주공이 친구를 선언할 수 있다.    
- To do
    - Player가 돌아가면서 카드를 내는 부분을 구현하기 위해 player list를 circular linked list로 변경 예정 -> 마땅 한 라이브러리를 못찾아서, 직접 scala를 사용하여 utility 클래스를 구현해볼 생각
    - Java로 실행 가능한 파일 생성하는 법 알아보기 (jar?) (continued)
- Issues
    - 구현된 내용을 문서화해주는 툴이 있으면 좋겠다… IntelliJ의 UML Diagram 생성 기능이 있다는데 찾을 수가 없다. (continued)
    - 몇몇 메소드들에 대한 테스트 케이스가 작성되지 않았다. 메소드가 테스트하기에 적합해보이지 않는데, 차차 개선 방안을 생각해보며 일단 기능 구현에 집중해보기로 했다. (continued)

<br><br>

## 확장 방향에 대해서

### Java GUI
- Swing 프레임워크를 사용하여 현재 텍스트로만 입출력되고 있는 프로그램을 GUI 프로그램으로 확장하는 방향

### Socket
- Socket 통신을 이용하여 여러 개의 콘솔로 통신해가면서 멀티로 게임을 진행할 수 있도록 확장하는 방향

### Android
- 현재 Java로 구현된 소스를 Kotlin으로 이식하여 안드로이드에서 구동할 수 있도록 확장하는 방향