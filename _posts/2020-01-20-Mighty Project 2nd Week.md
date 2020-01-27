---
title: "마이티 프로젝트 - 2주차"
excerpt: "마이티 프로젝트 2주차 진행상황"

categories:
    - Mighty Project
tags:
    - Mighty
    - Document
last_modified_at: 
---

## 2주차 진행상황
![공약 선언하기](/Images/Contract.JPG)
- Done
    - 게임이 시작될 때 랜덤하게 받은 카드의 점수 합이 2점 미만일 경우 딜미스를 선언할지 여부를 플레이어에게 묻고, 딜미스 선언시 게임이 다시 시작될 수 있도록 하였다.
    - 공약 선언의 기본 기능을 구현하였다.
        - 특정 플레이어가 공약을 선언하고, 그 후 남은 플레이어가 아무도 공약을 선언하지 않으면 해당 플레이어가 주공이 된다.
        - 아무도 공약을 선언하지 않을 경우, 공약의 최소 점수(기본은 13)가 1씩 내려간다.
        - 잘못된 공약을 입력할 경우 안내 문구를 출력하고 다시 공약을 선언하도록 했다.    
- To do    
    - Java로 실행 가능한 파일 생성하는 법 알아보기 (jar?) (continued)
    - 공약으로 노기루를 선언할 수 있도록 확장 필요
    - 공약 최소 점수가 1보다 내려가지 않도록 예외 처리 필요
    - 다음주는 공약 기능을 마무리한 후, 주공이 3장의 카드를 받아 패를 갱신하고 친구를 선언하는데까지 구현이 목표.
- Issues
    - 구현된 내용을 문서화해주는 툴이 있으면 좋겠다... IntelliJ의 UML Diagram 생성 기능이 있다는데 찾을 수가 없다. (continued)
    - 몇몇 메소드들에 대한 테스트 케이스가 작성되지 않았다. 메소드가 테스트하기에 적합해보이지 않는데, 차차 개선 방안을 생각해보며 일단 기능 구현에 집중해보기로 했다. (continued)


<br><br>

## 구현 중에 무슨 일이 있었나?

### 싱글톤의 함정
마이티에는 공약을 선언하는 과정이 있다. 플레이어들이 돌아가면서 본인의 패를 보고 기루(가장 높은 우선순위를 갖는 카드)와 본인이 달성할 점수를 선언하는데, 가장 높은 점수를 선언한 사람이 주공이 된다. 주공은 친구를 한 명 정하고, 본인이 선언한 점수 만큼의 점수 카드를 얻어야 게임에서 승리할 수 있다. 이번주에는 이 공약 선언하는 부분을 구현하려고 했다.

처음 작성된 Contract 클래스의 모습은 이랬다.

```java

public class Contract {
    private CardSuit giru;
    private Integer score;

    public Boolean bid(CardSuit suit, Integer score, Rule rule) {

        // 현재 공약 보다 낮은 점수는 공약할 수 없다.
        // Joker는 기루로 선언될 수 없다.        
        if ((this.score >= score) || (suit == CardSuit.JOKER)) { 
            return false;
        }

        // 현재 설정된 최소 공약 점수보다 낮은 점수로는 공약할 수 없다.
        if (score < rule.minScore) { 
            return false;
        }

        this.giru = suit;
        this.score = score;
        return true;
    }

    public Contract() {
        this.giru = CardSuit.SPADE;
        this.score = 13;
    }
}

```

bid 메소드는 공약이 선언될 때 실제 Contract 객체의 값을 바꿔주기 위한 메소드이다. 나는 원래 한 게임엔 하나의 Contract 객체가 존재해서, 플레이어가 공약을 선언할 때마다 적절한 공약인지를 체크하여 Contract 객체의 값을 바꿔주려고 했다. 그런데 막상 좀 더 구현을 시작하니, 공약을 선언하기 위한 세부적인 룰(ex. 선언할 수 있는 공약의 최소 점수)을 위해선 Contract 객체가 현재 게임의 진행상황을 알아야하고 (공약의 최소 점수는 별도의 Rule 객체가 관리하고 있다), 어떤 플레이어가 공약을 선언했는지는 어느 객체가 관리할 것인가 하는 등의 문제가 소소하게 거슬리기 시작했다. 원래 내가 원했던 Contract 객체는 선언된 공약의 데이터를 저장하고 그걸 Rule 객체한테 알려줘서 게임의 승패를 판단하는 역할만 했어야 했는데...

그러다 '굳이 Contract가 하나일 필요가 있나?' 하는 생각이 들었다. 생각해보면 플레이어마다 각자 공약을 선언할 수 있는데? 각자 선언한 공약이 있으면 Rule 클래스에서 공약을 비교해서 현재 규칙에 맞게 판단해줄 수도 있고, 각 플레이어의 Contract 인스턴스로 해당 플레이어가 공약을 선언했는지 판단도 할 수 있었다. 그래서 Contract 객체가 이렇게 바뀌었다.

```java

public class Contract {
    private CardSuit giru;
    private Integer score;
    private Boolean isDeclared;

    public void set(CardSuit suit, Integer score) {
        this.giru = suit;
        this.score = score;
        this.isDeclared = true;
    }

    @Override
    public String toString() {
        if (this.isDeclared) {
            return this.giru.toString() + " " + this.score.toString();
        } else {
            return "선언되지 않았습니다.";
        }
    }

    public Contract() {
        this.giru = CardSuit.JOKER;
        this.score = -1;
        this.isDeclared = false;
    }
}

```

이제 더 이상 Contract 클래스는 Rule 객체에 의존하지 않는다! 덤으로 본인이 선언된 공약인지를 판별할 수도 있게 되었다. 아직 공약 기능이 완전히 구현되지 않아 변화가 아주 작지만, 결과는 아주 만족스럽다. Contract 클래스를 사용하는 부분의 코드는 좀 더 깔끔해졌다.

회사에서 다루는 소스가 싱글톤의, 싱글톤에 의한, 싱글톤을 위한 코드라서 자꾸만 모든 객체는 다른 객체에 접근 가능하고 수정할 수 있는 쪽으로만 생각하게 된다. 가능한 책임이 명확한 클래스, 특정 클래스의 데이터에 직접 접근하여 수정하지 않을 것, 슈퍼 파워한 전역 싱글톤 객체의 사용을 자제할 것 그런 부분들에 의식적으로 주의를 기울이려하지만, 조금만 늘어져도 자꾸 익숙한 방향으로 코드를 작성하게 되는 것 같다. 연습이 많이 필요한 부분이다. 