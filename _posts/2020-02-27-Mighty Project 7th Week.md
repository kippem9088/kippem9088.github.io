---
title: "마이티 프로젝트 - 7주차"
excerpt: "마이티 프로젝트 7주차 진행상황"

categories:
    - Mighty Project
tags:
    - Mighty
    - Document
last_modified_at:
---

## 지난 이야기
Ruby를 업데이트하다가 버전이 꼬이기 시작하면서 한동안 jekyll의 serve 기능을 사용할 수 없었다. Ruby 버전을 올렸더니 기존에 사용하던 일부 파일에서 해당 버전 이하를 요구해서 다운 그레이드를 시도하다가 또 무언가 꼬이고... 결국 Ruby를 한 번 삭제한 후 낮은 버전으로 다시 받고, 여전히 따라오는 에러 메세지들을 무시하며 살다가 연차의 힘을 빌어 마음먹고 해결했다. 과정을 찍어뒀으면 좋았으련만 이제 와서는 무엇이 문제였는지 잘 기억이 나지 않아서 기록을 제대로 못남기는 것이 아쉽다. Ruby와 Gem과 Jekyll에 대해서 잘 모르기 때문에 매번 구글의 힘을 빌어 눈에 보이는 문제만 치우는 식인데... 맘잡고 Jekyll document 정도는 읽어볼 필요가 있을 것 같다.


글을 쓰진 못했지만 프로젝트 자체는 꾸준히 진행하고 있었다. 회사가 정신없던 일주일 정도는 쉬기도 했지만... 현재는 Socket programming을 사용해서 기존의 Java 코드를 server로 전환하고, WPF로 client 프로그램을 제작해서 통신하는 기능까지 구현이 되었다. Socket조차도 처음 사용해보기 때문에 구글신없이는 한 발짝도 나아가기가 힘들다. 뭐든 기초가 중요하다는 것을 실감하며 하고 있다.


<br>

## 7주차 진행상황
- Done
    - View-ViewModel 구조를 Window Client에 적용
    - Client에서 Server로 플레이어 이름을 보내면, 이후 5명의 플레이어가 전부 참가할 때까지 Client는 대기하며 계속 Player 정보를 Server로부터 받아 업데이트
- To do
    - Client의 UI Thread와 Background에서 Server와 통신할 Thread가 분리되어야 함
    - Client가 종료되었을 때, 해당 Player가 게임에서 나갔음을 Server에서 인지하고 Player를 예외처리 해주어야함
    - Client에서 카드를 그릴 수 있도록 하고, 카드를 나눠받는 부분 구현
    - Java로 실행 가능한 파일 생성하는 법 알아보기 (jar?) (continued)
- Issues
    - 구현된 내용을 문서화해주는 툴이 있으면 좋겠다… IntelliJ의 UML Diagram 생성 기능이 있다는데 찾을 수가 없다. (continued)
    - jekyll serve 실행 시 아래와 같은 에러가 계속 발생중
        - Conversion error: Jekyll::Converters::Scss encountered an error while converting 'assets/css/main.scss': Invalid CP949 character "\xEB" on line 17

