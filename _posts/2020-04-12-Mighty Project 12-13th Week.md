---
title: "마이티 프로젝트 - 12-13주차"
excerpt: "마이티 프로젝트 12-13주차 진행상황"

categories:
    - Mighty Project
tags:
    - Mighty
    - Document
last_modified_at:
---

각자의 일정으로 인해 조금씩 미뤄지다가 급 큰 일이 터지는 바람에 다시 2주치 업데이트의 날이 돌아왔습니다.

## 12-13주차 진행상황
- Done
    - 중구난방으로 작성중이던 Commit Log 규칙을 정했다.
    - 나눠받은 10장의 카드를 표시할 수 있도록 Client에 UI를 구현했다. 아직은 '다이아몬드 10' 요렇게 텍스트 기반으로만 표시되는 정도.
    - Server쪽에선 ExecutorService를 이용해서 각 Client의 메세지 수신 Thread가 아닌 별도의 Thread에서 game 진행이 이루어질 수 있도록 수정되었다.
- To do    
    - Java로 실행 가능한 파일 생성하는 법 알아보기 (jar?) (continued)
- Issues
    - 구현된 내용을 문서화해주는 툴이 있으면 좋겠다… IntelliJ의 UML Diagram 생성 기능이 있다는데 찾을 수가 없다. (continued)

<br><br>

## 짧은 넋두리

소켓 통신이라는 것이 추가되면서 점점 코드가 복잡해져만 가고, 클래스간 역할이 어때야하는지에 대한 고민도 점점 커져간다. 이제 딜미스를 체크하는 기능을 구현해야할텐데, 기존에는 Game class에서 Rule class에게 각 Player의 CardList를 넘겨주고 딜미스인지 여부만 체크하면 됐지만, 이제는 언제 어디서 누가 각 Client에게 딜미스를 선언할 지 물어볼 것인가에 대한 고민을 해야한다. 현재 Client와의 통신을 담당하고 있는 Main Server에서 직접 딜미스를 체크하고 물어보면 가장 간단하겠지만... 과연 Main Server가 그걸 다 아는게 맞는가 싶기도 하고. 설계에 대한 고민과 구현 사이에서 줄타기를 잘해야할 것 같다. 정 안되면 나중에 리팩토링 하면 되지 뭐.