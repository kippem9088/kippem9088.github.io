---
title: "마이티 프로젝트 - 결산"
excerpt: "마이티 프로젝트 결산 포스팅"

categories: 
    - Mighty Project
tags: 
    - Mighty
    - Document
last_modified_at: 
---

이 마이티 프로젝트는 회사 동기들과 '각자 만들고 싶은 프로그램을 하나씩 만들어보자!' 라는 주제로 시작한 프로젝트였다. 처음 정해졌던 기간은 3개월이었지만, 회사가 급박하게 돌아갔던 시기가 조금 있어서 약 한 달 정도 지연된 4월 말이 마감일로 정해졌다.

첫 시작은 단순히 '콘솔로 플레이 가능한 마이티를 만들어보자' 였지만, 도중에 소켓을 이용하여 서버와 클라이언트가 통신하며 플레이 가능한 구조로 만드는 것으로 목표가 확장되었다. 소켓 통신을 배워보자는 마음으로 솔직히 만만히 보고(?) 시작했던 확장이었지만, 생각보다 클라이언트의 UI를 구현하는데 많은 공을 들여야만 했다. (그리고 막상 디자이너 없이 막눈으로 짜다 보니 여전히 UI는 심각하게 좋지 않다^^) 덕분에 지난 2주간 정신없이 달려 겨우겨우 게임의 끝자락까지 구현했지만, 여전히 게임의 룰을 완전히 구현해내진 못한 미완성품으로 마무리를 짓게 되었다.


## Server

### 환경
- 사용언어: JAVA (IntelliJ)
- Test FrameWork: JUnit5
- Repository: [GhostFriend](https://github.com/kippem9088/GhostFriend)

### 목표
- JAVA 언어에 익숙해진다. 
- 객체 지향적인 코드를 작성한다.
- Unit Test를 작성한다.

### 결산
Local IP를 통해 통신하는 Socket Server를 열어 Mighty Game을 컨트롤하는 프로그램이다. 'Command-Data1/Data2/...' 식의 문자열 스트림을 하나의 통신 단위로 사용한다. 아직 JVM 환경에서만 실행 가능하므로 실제로 동작한 Log 기록을 첨부했다.

![Server 동작 로그](/Images/ServerLog.PNG)


주요 기능의 Class Diagram은 아래와 같은 모양이다.

ClientControl은 각 Client로부터의 문자열을 읽어 Command 단위로 분리한 후 각 Command에 맞게 Game/MainServer의 메소드를 호출하는 역할을 한다.

현재 역할 구분이 가장 애매한 것은 Game Class와 MainServer Class인데... 최초 의도는 Game Class에서는 game이 동작하는 business logic에 관한 것만을 관리하고, MainServer는 Player List를 저장하고 있다가 Player에게 command를 전체적으로 보내야할 때라던지 하는 통신 부분을 관리하려고 했다. 그런데 기능을 하나 둘 붙여나가다 보니 Game Class의 Logic에서 Player와 통신할 일이 자꾸만 생겼고, 결국엔 MainServer를 계속 부르게 되면서 둘 사이의 관계가 모호해졌다. 이 부분은 조만간 구조를 정리할 필요가 있다.

그 외에는 Mighty 라는 게임의 로직을 구현하기 위해 역할을 분담하는 Class들이다. 

![Server Class Diagram](/Images/ServerClassDiagram.PNG)


객체 구조에 대한 설계 외에 중점을 둔 부분은 단위 테스트 작성이다. Client와 직접 통신을 하는 Game Class 외의 Base Package Class 들은 Test Case를 작성하였다.

![Server Test Converage](/Images/ServerTestConverage.PNG)

Client 개발과 병행하다보니 Server 쪽의 기능을 적당히 돌아가게 구현해서 통신이 적절한지를 먼저 테스트한 뒤, 게임의 기능을 적합하게 구현하고 있는지 Test Case로 검증해볼 일이 많았다. 랜덤하게 카드를 분배하는 것을 전제로 게임을 하다보니 원하는 극단적인 케이스를 마주칠 일이 별로 없었는데, Test Case를 통해 사전에 방지했던 버그가 꽤 있었다. 


## Client

### 환경
- 사용언어: C# (Visual Studio 2017)
- FrameWork: MVVM
- Repository: [GhostFriend_WindowClient](https://github.com/kippem9088/GhostFriend_WindowClient)

### 목표
- ?

### 결산
Client 프로그램은 뚜렷하게 무엇을 해보겠다고 시작한 프로젝트는 아니었다. Socket Server를 통해 동시에 여러명이 Server와 통신해가면서 게임을 플레이해볼 수 있었으면 좋겠다고 생각해서 시작한, 어찌보면 통신만 가능하면 어떻게 생겨도 상관없다는 마음으로 가볍게 시작했는데... 기능을 하나, 둘 추가하며 UI 디자인의 어려움을 절실히 체감하고... 기능을 쉽게 추가하려고 보니 MVVM을 적용시키기 시작하고... 그랬더니 Server Listener 단에서 ViewModel을 직접 접근하기가 뭐하니 Event 구조를 공부하기 시작하고... 화면이 가변해야하니 UserControl을 하나 둘 추가하고... Binding을 하다보니 특정 컴포넌트가 제대로 업데이트가 되지 않아서 또 Binidng Property를 검색하고... 

한 마디로 별 일 안하겠다고 다짐하고 쉽게 시작했는데 점점 눈덩이처럼 일이 커져만 가서 어찌보면 Server보다도 힘들게 한 프로젝트가 되었다.

![Client Forder List](/Images/ClientFolders.PNG)

힘은 많이 들었지만 사실 'View를 ViewModel에 Binding한다', 'Socket 단에서 들어온 command는 EventHandler를 통해 ViewModel에 연결한다'는 두 원칙을 지키면서 일어나는 문제들을 처리하는 과정이 프로젝트 내내 반복되었다.