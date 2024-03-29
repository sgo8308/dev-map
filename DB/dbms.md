<details>
<summary>어떤 기준으로 DB를 선택해야할까?</summary>
<br>
보통 회사들은 MySQL과 같은 범용 DBMS를 먼저 선택하고, 사용량이나 데이터의 크기가 커지면 
일부 도메인 또는 테이블의 데이터만 전용 DBMS로 이전해서 확장하는 형태를 선택한다.

왜 범용 DBMS 먼저 선택할까?

전용 DBMS는 어떤 기준으로 선택할까?

---

백은빈, 이성욱, Real MySQL 8.0, 2쇄, 위키북스, 저자 서문, 2022
</details>

<details>
<summary>RDBMS와 NoSQL의 차이점에 대해 설명해주세요</summary>
<br>
NoSQL은 non SQL 도는 not noly SQL이라는 의미이다.

RDBMS은 일반적인 용도로 사용되고 NoSQL은 수많은 종류가 있으며 제각기 특화된 분야가 있다.

RDBMS느 Scale-Out이 어렵다.

왜냐하면

1. 자체적으로 지원하지 않는다.

   오래된 역사를 갖고 있는 만큼 관계형 데이터베이스가 처음 나온 시절에는 scale-out이라는 것을 생각조차 못했고(매우 비싸고 큼) 자연히 scale-up만을 지원하는 방식으로 발전해왔다.

2. Join 연산이 많은데 관련 테이블이 여러 서버로 퍼질 경우 Join을 수행하기가 힘들다.
3. 분산 서버 간에 트랜잭션을 지원하기가 어렵다.

NoSQL은 Scale-Out이 쉽다.

왜냐하면

1. 자체적으로 지원해준다.
2. 트랜잭션과 Join에 관한 기능을 잘 지원안하는 것들이 많기 때문에 구현이 쉽다.

### 관계형 모델

- 장점

  범용적으로 다양한 케이스에 대해서 무난하게 사용할 수 있다.

  선언형 방식의 SQL로 직관적으로 데이터를 조작할 수 있으며 데이터베이스가 쿼리를 최적화해줄 여지가 많다.

  1대1,1대다, 다대다 모든 관계를 표현할 수 있고 Join의 성능이 좋다.

  스키마대로 데이터 입력하는 것을 강제하기 때문에 잘못된 데이터가 들어오는 것을 방지할 수 있다.


- 단점

  하나의 데이터 셋(예를 들어 프로필)을 갖고오기 위해 여러 테이블에 join해야 하고 이는 성능저하로 이어진다.

  데이터 저장시 관련 데이터셋을 여러 테이블에 나누어 저장하기 때문에 성능이 떨어진다.

  새로운 칼럼을 추가하는 등의 작업이 테이블 전체에 영향을 미치므로 운영 환경에서 함부로 변경하기 어렵다.

  데이터를 테이블 스키마에 맞추어 넣어야 해서 유연하지 않다.


### 문서 모델

마치 문서 하나를 저장하듯 관련 데이터를 한대 모아 저장하는 모델.

- 장점
    
    주로 일대다 관계(트리구조)로 되어 있거나 레코드간 관계가 없어 다대다 형태가 생기지 않을 때 사용하면 좋다.
    
    데이터의 구조를 한 눈에 알 수 있고 관련 데이터를 한 번에 불러 올 수 있어 성능이 좋다.
    
    예를 들어 프로필같은 데이터가 있다.
    

- 단점
    
    다대다 구조로 가게 될 경우 조인이 필요하다. 조인을 지원하지 않는 DB도 있고 지원하더라도 관계형 DB보다 코드가 복잡하고 성능이 떨어진다.
    
    만약 조인을 사용하지 않고 비정규화로 가게 된다면, 일관성을 유지하기 위해 중복되는 데이터를 항상 함께 수정해야 되고 이는 성능저하와 복잡도 향상으로 이어진다.
    
    중첩 항목을 바로 참조할 수 없기 때문에 중첩이 깊어질수록 조회하기가 어려워진다. 예를 들어 사용자 251의 교육 항목의 학교 항목의 이름. 이런 식으로 타고타고 들어가야 한다. 하지만 너무 깊어지지 않으면 크게 문제되지는 않는다.
    

### 그래프 모델

데이터를 노드로, 데이터 사이의 관계를 간선으로 표현한 모델

- 장점
    
    깊고 복잡한 관계를 가진 데이터 모델에 적합하다. 간결한 쿼리로 복잡한 관계를 가진 조회를 쉽게 수행할 수 있다. 예를 들어 ‘미국에서 유럽으로 이민 온 모든 사람들의 이름 찾기’ 같은 쿼리가 있다. 만약 관계형 모델이라면 Person 테이블에 도시에 관한 값만 갖고 있을 것이다. 그리고 이 쿼리를 수행하기 위해 도시 → 주 → 대륙 으로 관계를 타고 들어가서 미국에서 태어났는지를 알 수 있다. 이렇게 관계를 깊게 타고 들어갈 때 미리 JOIN 문으로 지정하기가 힘들다. With Rescursive문과 같은 재귀적인 문법을 제공하는 관계형 모델일지라도 그래프 모델에 비해 훨씬 긴 쿼리가 필요하다.
    

- 단점
    
    범위 검색을 수행하기가 힘들다.
    

### Key-Value 모델

Map과 동일하게 Key에 Value를 매핑하고 Key를 통해 Value를 찾을 수 있는 모델

- 장점
    
    단순하기 때문에 사용성이 좋다.(SQL은 학습이 필요함)
    
    Hash를 통해 값을 바로 읽으므로 속도가 빠르다.
    
    분산 환경에서 수평적으로 확장하기 좋다.
    

- 단점
    
    Key를 통해서만 값을 읽을 수 있다.
    
    범위 검색 등의 복잡한 질의가 불가능하다.
    

### Wide Column

- 장점
    
    Row마다 다른 스키마를 가질 수 있어 유연하다.
    

---

[https://www.mongodb.com/ko-kr/nosql-explained/nosql-vs-sql](https://www.mongodb.com/ko-kr/nosql-explained/nosql-vs-sql)

데이터 중심 애플리케이션 설계 34p, 53p

****백엔드 개발자를 위한 한 번에 끝내는 대용량 데이터 & 트래픽 처리 초격차 패키지 Online. Part3 ch01-2****

[https://www.samsungsds.com/kr/insights/1232564_4627.html](https://www.samsungsds.com/kr/insights/1232564_4627.html)

</details>