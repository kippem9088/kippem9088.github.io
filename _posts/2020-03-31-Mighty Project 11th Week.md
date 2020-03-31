---
title: "마이티 프로젝트 - 11주차"
excerpt: "마이티 프로젝트 11주차 진행상황"

categories:
    - Mighty Project
tags:
    - Mighty
    - Document
last_modified_at:
---

## 11주차 진행상황
- Done
    - 통신 구조를 변경하여 아래 이슈를 해결하였다.
        - 마지막으로 게임에 참여한 Client는 바로 모든 플레이어가 게임에 참여하였다는 메시지를 받기 때문에 제대로 Player List를 업데이트하지 못하는 문제가 있다.    
    - Server-Client간의 통신을 단순히 String만 왔다갔다 구조에서 command와 data와 구분되게 전달되는 구조로 변경
    - Server 쪽에서 command를 보내거나 받을 때 간단하게 txt 파일에 로그를 남기는 기능을 추가
    - Server 쪽에 MainServer class를 추가하여 player의 InputStream, OutputStream 리스트를 관리하면서 필요할 때 broadcast를 할 수 있도록 구조를 정리하였다.
- To do    
    - Card를 분배하고 이를 Client에 전달하는 타이밍을 언제로 잡을 지가 고민이다. 분명 Server단에서 해야하는데, 현재 Server는 Start해주는 Main Thread 외엔 각 Client별 Thread 밖에 없고, Game에서 주체적으로 무언가를 할 수 있는 방법이 없다. 구조 고민을 해봐야 한다.
    - Java로 실행 가능한 파일 생성하는 법 알아보기 (jar?) (continued)
- Issues
    - 구현된 내용을 문서화해주는 툴이 있으면 좋겠다… IntelliJ의 UML Diagram 생성 기능이 있다는데 찾을 수가 없다. (continued)

<br><br>

## 구조 변경에 대한 간단한 기록

먼저 Socket을 이용하여 String을 주고 받아 컨트롤한다는 단순한 발상에서 뻗어나간 초기의 통신 상태를 보자. 그림을 단순하게 하기 위해 최대로 참여할 수 있는 Player는 2명이라고 가정했다.

![초기 구조](/Images/Original_Structure.JPG)

어떻게 생각하면서 코드가 짜여져 나갔는지를 알 수 있을 만큼 구조는 아주 단순하다.

1. Client에서 게임에 참여하면, 'Join Game' 커맨드를 보내고 뒤이어 Player의 이름을 전송한다.
2. 새로운 Player가 참여하면 Server는 모든 Client들에게 'New Player Joined' 커맨드를 전송한다.
3. 게임에 참여한 Client는 계속 대기하고 있다가, Server로부터 Player가 변동되었다는 정보를 받으면 'Ask Player Info' 커맨드를 보내고, Server로부터 Player Info를 받아 UI를 업데이트한다.
4. 만약 모든 Player가 게임에 참여하면 Server는 'All Player Joined' 커맨드를 보내고, Client들은 더 이상 Player Info를 업데이트하지 않는다.

가장 먼저 눈에 띈 이슈는 Client가 연속으로 들어온 Command를 구분할 수 없기 때문에 발생했다. 모든 Player가 참여했을 때 Server가 'New Player Joined' 커맨드와 'All Player Joined' 커맨드를 연속해서 날렸기 때문에, 연속된 커맨드를 구분할 줄 몰랐던 Client는 'New Player Joined All Player Joined' 커맨드가 들어온 것으로 해석하고 처리할 수 없는 커맨드이기 때문에 아무 일도 하지 않았다. 따라서 Client가 Player List를 업데이트하지 못했다.

가장 먼저 떠오른 해결책은 Server가 커맨드를 보냈을 때, Client가 그 커맨드를 다 처리할 때까지 Server가 기다려주는 것이었다. 마치 Windows의 SendMessage가 동작하는 방식처럼. 모든 커맨드가 순차적으로 처리되어야할 경우에 많이 사용했던 해결책이다. 유사한 코드를 만들어서 바로 적용해보았는데 왠걸, Server가 멈춰버렸다. 그랬다. Multi thread에서의 Send Message는 언제나 위험하다. Client 1에 대한 thread가 계속 Client 1으로부터의 입력을 기다리고 있는데, Client 2의 Thread에서 새로운 Player가 참여했다는 커맨드를 보내면 Client 1의 응답은 Client 1의 Thread로 갈 지 Client 2의 Thread로 갈 지 알 수가 없었다. (실험했을 때는 언제나 Client 1의 Thread로 가서 Client 2의 Thread가 필요한 응답을 받지 못하고 멈춰버렸다.) 이후 응답을 받을 수 있도록 입출력에 대한 구조를 개선해보려는 시도를 조금 더 했지만 아직 내 지식 범위내에서는 쉽사리 개선할 수 없겠다는 결론을 내렸다.

