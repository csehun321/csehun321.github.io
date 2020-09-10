---
layout: post
title:  "코딩테스트 연습 <스택/큐> -프린터"
date:   2020-09-20 18:37:00
author: csehun321
categories: 프로그래머스
tags:	java, queue
cover:  "/assets/instacode.png"
---
### 문제 설명

- 인쇄 대기목록의 가장 앞에 있는 문서(J)를 대기목록에서 꺼냅니다.
- 나머지 인쇄 대기목록에서 J보다 중요도가 높은 문서가 한 개라도 존재하면 J를 대기목록의 가장 마지막에 넣습니다.
- 그렇지 않으면 J를 인쇄합니다.

### 제한사항
- 현재 대기목록에는 1개 이상 100개 이하의 문서가 있습니다.
- 인쇄 작업의 중요도는 1~9로 표현하며 숫자가 클수록 중요하다는 뜻입니다.
- location은 0 이상 (현재 대기목록에 있는 작업 수 - 1) 이하의 값을 가지며 대기목록의 가장 앞에 있으면 0, 두 번째에 있으면 1로 표현합니다.

### ...
 예를들어서 각 문서의 `priorities`가 `[2, 1, 3, 2]`이고 `location`이 1이라면
 `location`은 인덱스를 우선순위가 1인 2번째 문서가 몇 번째로 출력되는지 반환하는거다.

 첫 번째 문서는 우선순위가 2고 뒤에 우선 순위가 더 높은 문서가 있으니 맨 뒤로 옮겨진다...

 `[1, 3, 2, 2]`

 그 다음도 마찬가지

 `[3, 2, 2, 1]`

 모든 과정이 끝나면 이렇게 우선순위 순서대로 출력되게 된다능...
 원하는 문서가 네번째로 출력됐으니 리턴되는 값은 4일 거임.

### 코드

    import java.util.*;
    class Solution {
      public int solution(int[] priorities, int location) {
          PriorityQueue<Integer> pri = new PriorityQueue<>();

PriorityQueue는 우선순위 큐다.
큐는 먼저 들어온 놈이 먼저나가는게 국룰인데 우선순위 큐는 들어온 순서는 상관없고
우선순위가 높은 요소가 먼저 나간다.

우선순위는 숫자가 작을 수록 높은거다. 근데 이 문제는 높은애가 숫자가 더 크니까 반대로 나가야한다.

          int answer = 0;

          for (int i = 0; i < priorities.length; i++){
              priorities[i] = 10 - priorities[i];
          }

우선순위가 1부터 9까지라고 하니 10에서 빼면 우선순위 큐 규칙에 따라서 차례대로 나가겠지
근데 굳이 이렇게 안 해도 된다고 함.

    PriorityQueue<Integer> pri = new PriorityQueue<>(Collections.reverseOrder());

이렇게 하면 우선순위가 반대로 된다고 한다.

          for(int priority : priorities) {
              pri.add(priority);
          }

`priorities`배열의 값을 우선순위 큐에 넣는다. 자동으로 정렬될 것이다.

          while(!pri.isEmpty()){
              for(int i = 0; i < priorities.length; i++) {
                  if(pri.peek()==priorities[i]) {
                      pri.poll();
                      answer++;
                      if(location == i) {
                          pri.clear();
                          break;
                      }
                  }
              }
          }
          return answer;
      }
    }

마지막으로 우선순위 큐에 들어있는 값과 인풋배열에 들어있는 값을 차례대로 비교한다.
우선순위가 높은애들부터 앞에 있으니까 맨 앞에 있는애를 `peek`해서 비교하고 같은애가 나오면 `poll`해서 방출한다.
이러면 차례대로 나가게 된다. 그리고 한 놈 나갈때마다 `answer`값을 올린다.
인풋 배열은 그대로 유지되니까 `i`가 `location`과 같을 때 큐의 맨 앞과 배열의 값이 같으면 정답인거다.
while문은 빠져나와야 하니까 큐는 `clear`로 비워주고 끝낸다.

딱 문제 보면 뭔가 쉬워보이고 큐 안 써도 좀 어떻게 해보면 될것처럼 생겼는데 저 간단한 생각이 잘 안 난다. 내가 너무 좆밥인걸 실감한다.
