### assert 예약어를 설명해주세요.
    
어떤 불리언 식이 참이 아니면 더 이상 진행하지 못하게 하고 참이면 진행하게 해서 신뢰성 있는 개발을 도와주는 것. 

예를 들어 어떤 메소드를 사용할 때 인자값으로 옳바른 값이 오지 않았다면 더 이상 진행시키지 않고 에러를 내뱉도록 하고 싶다면 다음과 같이 하면 된다.

```java
private String calculateAge(int age){
    assert age > 0;
    //나이를 계산하는 로직 실행
}
```

이런 식으로 사용할 경우 만약 나이가 0살을 넘지 않는다면 AssertionError가 발생한다.

위와 같이 사전에 인자를 체크하는데 사용할 수도 있고, 메소드가 다 실행된 후 결과값을 체크하는데 사용할 수도 있다.

개발시에만 사용되고 실제 production code에는 빠지기 때문에 assert 안에 실제로 프로그램 흐름에 필요한 메소드 같은 것들 집어넣으면 실제 운영 때 이 메소드가 실행이 안되서 문제가 생길 수 있다.

따라서 이렇게 하지 말고

```java
assert doSomething(); // doSomething이 프로그램 실행 흐름 상 꼭 필요한 메소드
```

이렇게 하면 된다.

```java
boolean bool = doSomething();
assert bool;
```
  

### UUID란?
    
Universal Unique Identifier의 약자로 전세계적으로 유일한 값이라는 의미이다. 물론 겹칠 확률이 완전히 0은 아니지만 0으로 봐도 무방할 정도로 겹칠 확률이 낮다.

32개의 문자와 4개의 대시('-')가 추가된 랜덤하게 생성된 128bit가 문자열이라고 보면 된다.

---

[https://en.wikipedia.org/wiki/Universally_unique_identifier](https://en.wikipedia.org/wiki/Universally_unique_identifier)

### ISO 8601이란 무엇이고 무슨 문제를 해결해줄까?
    
    날짜와 시간 관련 데이터를 주고 받을 때 사용되는 국제 표준이다. 여러 나라나 시스템이 데이터를 주고 받을 때 서로 통일되지 않아 오류가 나는 것은 방지하기 위해 제정됐다.
    
    이 표준은 ISO라 불리고 현재 최신 버전은 ISO 8601이다.
    
    형태
    
    2017-03-16T17:40:00+09:00
    
    - 날짜 : 년-월-일
    - T : 날짜 뒤에 시간이 오는것을 표시해주는 문자
    - 시간 : 시:분:초의 형태로 나와있으며 프로그래밍 언어에 따라서 초 뒤에 소수점 형태로 milliseconds가 표시
    - Timezone Offset : 시간 뒤에 ±시간:분의 형태로 나와있으며 UTC기준 시로부터 얼마만큼 차이가 있는지를 나타냅니다.
       현재 위의 예시는 한국시간을 나타내며 UTC기준 시로부터 9시간 +된 시간임을 나타낸다.
    - 위 형태의 시간의 UTC시간은 2017-03-16T08:40:00Z이다. 뒤에 Z가 붙으면 UTC이고 +나 -가 붙으면 ISO 형식의 시간이다.
    
    ---
    
    [https://java119.tistory.com/24](https://java119.tistory.com/24)