---
title: "마이티 프로젝트 - 8주차"
excerpt: "마이티 프로젝트 8주차 진행상황"

categories:
    - Mighty Project
tags:
    - Mighty
    - Document
last_modified_at:
---

## 8주차 진행상황
- Done
    - Player 정보 업데이트 인터페이스 변경
    - 특정 Player의 Socket 연결에서 에러가 발생했을 경우, 해당 Player를 게임으로부터 제외시키도록 함
    - Game에 이미 5명의 Player가 참가했을 경우, 이후 들어오는 연결은 받지 않으며 에러 코드를 전송하여 해당 client에서 게임에 참가할 수 없도록 함
    - Thread Pool을 이용하여 서버와의 통신은 별도의 Thread에서 처리할 수 있도록 구현
- To do
    - Client에서 카드를 그릴 수 있도록 하고, 카드를 나눠받는 부분 구현
    - Java로 실행 가능한 파일 생성하는 법 알아보기 (jar?) (continued)
- Issues
    - 구현된 내용을 문서화해주는 툴이 있으면 좋겠다… IntelliJ의 UML Diagram 생성 기능이 있다는데 찾을 수가 없다. (continued)


<br><br>

## 구현 중에 무슨 일이 있었나?

### 언제 어떻게 정보를 요청할까?
처음 구현을 시작할 때는 5명의 플레이어를 전부 코드로 생성해주었기 때문에, 언제 어떤 플레이어가 게임에 참가할 지를 고민할 필요가 없었다. 그런데 Client 프로그램이 별도로 분리되면서 어떤 플레이어가 언제 참가할 지를 예측할 수가 없게 되었다. 언제 Server로부터 Player 정보를 받아와서 UI를 업데이트해주는 것이 좋을까?

가장 먼저 떠오른 생각대로 작성한 코드는 다음과 같다.

```cs

private void JoinGame()
    {
        SocketClient.Instance.StartConnection();
        GameControl.Instance.Join(PlayerName);

        WaitOtherPlayers();
    }

private void WaitOtherPlayers()
    {
        while (!GameControl.Instance.IsAllPlayersParticipatedIn())
        {
            String[] playersInfo = GameControl.Instance.ReceivePlayersInfo();

            for (int i = 0; i < playersInfo.Length; i++)
            {
                SetPlayerName(i + 1, playersInfo[i]);
            }
        }
    }

```

본인이 게임에 참가한 다음, 다른 플레이어가 5명 모두 참가할 때까지 While 문을 돌면서 지속적으로 Server에게 플레이어 정보를 요청한다. 2명의 플레이어로 테스트를 해보고 양쪽의 Client가 모두 잘 업데이트되는 것을 확인했다. 만족스러웠다.

문제는 Client 프로그램을 4개 실행한 후에 발견이 됐다. 옆에 Youtube를 틀어놓고 테스트를 하고 있었는데, 영상이 갑자기 버퍼링이 걸리고, Client 프로그램은 실행도 되지 않았다. 작업 관리자를 확인해보니 Server를 돌리고 있는 IntelliJ IDE의 메모리 사용량이 어마어마하게 치솟아 있었다. (6개월째 미루고 있는 컴퓨터 업그레이드의 욕망이 차올랐고) While문이 문제라는 건 바로 알 수 있었다.

그제서야 Client-Server간 통신 인터페이스를 더 제대로 구현해야한다는 걸 깨달았다. 기능이 동작하는 데만 주력한 나머지 성능에 대한 고려를 전혀 하지 않았던 것이다. Server에서 Client로 새 플레이어가 참가했음을 알릴 수 있는 인터페이스를 만들고, 다중 쓰레드로 구현되어있는 Server에서도 새 플레이어가 참가했을 때 다른 Client에게도 알릴 수 있도록 HashMap을 이용한 BroadCast 기능을 추가했다. 그래서 현재 최종 모습은 이렇게 바뀌었다.


#### Client
``` cs

private void JoinGame(object state)
    {
        SocketClient.Instance.StartConnection();
        GameControl.Instance.Join(PlayerName);

        String joinResult = SocketClient.Instance.ReceiveData();

        if (joinResult.Equals(GameParams.JOIN_SUCCESS))
        {
            WaitOtherPlayers();
        }
        else if (joinResult.Equals(GameParams.JOIN_FAIL))
        {
            AnnounceMessage("You cannot join the game");
        }            
    }

private void WaitOtherPlayers()
    {
        String serverCommand = SocketClient.Instance.ReceiveData();

        while (!serverCommand.Equals(GameParams.ALL_PLAYERS_ENTERED))
        {
            if (serverCommand.Equals(GameParams.JOIN_NEW_PLAYER) ||            serverCommand.Equals(GameParams.EXIT_PLAYER)) {
                String[] playersInfo = GameControl.Instance.ReceivePlayersInfo();

                for (int i = 0; i < playersInfo.Length; i++)
                {
                    SetPlayerName(i + 1, playersInfo[i]);
                }
            }

            serverCommand = SocketClient.Instance.ReceiveData();
        }
    }        

```


#### Server
```java

// Client로부터의 입력을 받는 부분
while (isConnected) {
    try {
        String commandParam = bufferedReader.readLine();

        if (commandParam.equals(GameParams.JOIN_GAME)) {
            String playerName = bufferedReader.readLine();
            player = game.addPlayer(playerName, printWriter);

            if (player == null) {
                sendText(GameParams.JOIN_FAIL);
            } else {
                sendText(GameParams.JOIN_SUCCESS);
                broadcast(GameParams.JOIN_NEW_PLAYER);

                if (game.isAllPlayersEntered()) {
                    broadcast(GameParams.ALL_PLAYERS_ENTERED);
                }
            }
        }
        else if (commandParam.equals(GameParams.ASK_PLAYERS_INFO)) {
            String playersInfo = game.getPlayersInfo(PLAYER_INFO_DELIMITER);
            sendText(playersInfo);
        }
    }
    catch (SocketException e) {
        e.printStackTrace();
        game.removePlayer(player);
        broadcast(GameParams.EXIT_PLAYER);
        isConnected = false;
    }
    catch (IOException e) {
        e.printStackTrace();
        isConnected = false;
    }
}

private void broadcast(String text) {
    synchronized (game.getPlayers()) {
        Collection<PrintWriter> collection = game.getPlayers().values();
        Iterator<PrintWriter> iter = collection.iterator();

        while (iter.hasNext()) {
            PrintWriter pw = iter.next();
            pw.println(text);
            pw.flush();
        }
    }
}

```

Client-Server 프로그램을 처음 다뤄보기 때문에, 하나하나가 난관이다. 버그를 만날 때마다 구조가 계속해서 바뀌고 있다. 알 수가 없으니 최대한 고치기 쉽도록 유연한 코드를 만들기 위해 노력하는 것 밖에 할 수 있는 게 없다. 이번 프로젝트에서 가장 크게 배우는 건 역시 항상 내가 예상치 못한 문제는 터질 수 밖에 없고 기한은 항상 예상한 것보다 늘어난다는 것인듯 싶다.