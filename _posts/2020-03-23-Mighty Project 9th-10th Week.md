---
title: "마이티 프로젝트 - 9-10주차"
excerpt: "마이티 프로젝트 9-10주차 진행상황"

categories:
    - Mighty Project
tags:
    - Mighty
    - Document
last_modified_at:
---

야근이 계속되어 진행률이 저조하므로 2주 분량 업데이트!ㅠ

## 9-10주차 진행상황
- Done
    - TrumpCard UserControl 생성해서 간단하게 Binding을 걸어봄
    - Server 코드의 묵은 Warning을 청소했다.
    - 모든 Player가 게임에 참가했을 경우, 각 플레이어에게 카드를 분배하고 이를 Client에게 알리는 것까지 구현    
- To do
    - 마지막으로 게임에 참여한 Client는 바로 모든 플레이어가 게임에 참여하였다는 메시지를 받기 때문에 제대로 Player List를 업데이트하지 못하는 문제가 있다.
    - Card를 분배하고 이를 Client에 전달하는 타이밍을 언제로 잡을 지가 고민이다. 분명 Server단에서 해야하는데, 현재 Server는 Start해주는 Main Thread 외엔 각 Client별 Thread 밖에 없고, Game에서 주체적으로 무언가를 할 수 있는 방법이 없다. 구조 고민을 해봐야 한다.
    - Java로 실행 가능한 파일 생성하는 법 알아보기 (jar?) (continued)
- Issues
    - 구현된 내용을 문서화해주는 툴이 있으면 좋겠다… IntelliJ의 UML Diagram 생성 기능이 있다는데 찾을 수가 없다. (continued)


<br><br>

## Warning을 해결하다 얻은 소소한 팁들

### 'while' loop replacable with enhanced 'for' loop inspection

HashMap에 대한 예제 코드에서 iterator를 사용하고 있어서 그대로 사용하고 있었는데, 그보단 Java 5에서 도입된 Enhanced For Loop을 사용해보는 것이 좋겠다고 제안해줬다.

```java

// 원본
public void distributeCards() {
    synchronized (players) {
        Set<Player> playerSet = players.keySet();
        Iterator<Player> iter = playerSet.iterator();

        while (iter.hasNext()) {
            Player player = iter.next();

            for (int i = 0; i < Rule.getNumOfCardsPerPerson(); i++) {
                player.receiveCard(deck.drawCard());
            }
        }
    }
}

// 수정 후
public void distributeCards() {
    synchronized (players) {
        Set<Player> playerSet = players.keySet();

        for (Player player : playerSet) {
            for (int i = 0; i < Rule.getNumOfCardsPerPerson(); i++) {
                player.receiveCard(deck.drawCard());
            }
        }
    }
}

```

### String concatenation '+=' in loop

[참고](https://help.semmle.com/wiki/display/JAVA/String+concatenation+in+loop)

Java의 문자열은 immutable이기 때문에, "+" 연산자는 결과값을 저장할 새로운 문자열을 생성한 후 재할당하는 방식을 사용한다. 당연하게도 문자열을 자주 변경하면 할수록 성능이 떨어질 것이다. 아래 예제처럼 반복문을 통해 문자열을 생성하는 경우 String을 그대로 사용하기보다는 StringBuilder나 StringBuffer를 사용하라고 한다. (StringBuffer의 경우 multi-thread 접근이 자주 있을 때 사용을 권장하고, 그렇지 않을 경우 StringBuilder가 overhead가 적다.)

```java

// 원본
public String getPlayersInfo(String delimiter) {
    String playersInfo = "";

    synchronized (players) {
        Set<Player> playerSet = players.keySet();
        Iterator<Player> iter = playerSet.iterator();

        while (iter.hasNext()) {
            playersInfo += iter.next().getName();
            playersInfo += delimiter;
        }
    }

    return playersInfo;
}

// 수정 후
public String getPlayersInfo(String delimiter) {
    StringBuilder playersInfo = new StringBuilder();

    synchronized (players) {
        Set<Player> playerSet = players.keySet();

        for (Player player : playerSet) {
            playersInfo.append(player.getName());
            playersInfo.append(delimiter);
        }
    }

    return playersInfo.toString();
}

```

### Synchronization on a non-final field 'players'

Java의 field에 사용되는 final 키워드는 entity를 한 번만 할당한다는 의미가 있다고 한다.