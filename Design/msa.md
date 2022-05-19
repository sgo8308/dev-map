### 공부해야 하는 이유는?
    
    여러 기업에서 MSA로 전환을 많이 하고 있다.
    
    따라서 MSA란 무엇인지 왜 전환을 하는지 등등을 이해할 필요가 있다.
    
### 정의는?
    
    하나의 어플리케이션을 스스로 동작하면서 독립적으로 배포가능한 작은 서비스들의 집합으로 만드는 방식. 각각의 서비스들은 네트워크를 이용해 통신한다.
    
    ---
    
    [https://martinfowler.com/articles/microservices.html](https://martinfowler.com/articles/microservices.html)
    
### 사용해야 하는 이유는?
    
    MSA가 아니라면 모놀리식 아키텍쳐를 사용하게 된다. 모놀리식 아키텍쳐는 MSA와는 반대로 어플리케이션을 하나의 큰 덩어리로 구축하는 일반적인 방식이다.
    
    이 방식은 다음과 같은 문제점이 있다.
    
    1. 작은 부분을 수정해도 전부 새로 컴파일하고 빌드해서 배포해야 한다.
    2. 특정 서비스 모듈의 변경의 여파가 다른 모듈까지 번질 수 있다.
    3. 특정 서비스 모듈만 확장하는 것이 불가능하다.
    
    MSA는 이러한 문제점들을 해결해주기 때문에 사용하는 것이 좋다.
    
    ---
    
    [https://martinfowler.com/articles/microservices.html](https://martinfowler.com/articles/microservices.html)
    
### 단점은?
    1. 네트워크를 이용해 서비스 통신을 하기 때문에 Latency로 인한 성능 저하가 있을 수 있다.
    2. 데이터가 여러 서비스에 걸쳐 분산되기 때문에 한번에 조회하기가 어렵고, 데이터 정합성 관리가 어렵다.
    3. 트랜잭션, 테스트의 복잡도가 증가한다.

### MSA 도입시 중요하게 고려해야 하는 부분은 무엇일까? ★
    1. 우리의 시스템이 정말로 MSA를 도입할 만큼 복잡한지 생각해봐야 한다. 
    충분히 복잡하지 않은 서비스에서는 MSA 도입이 오히려 생산성을 떨어트린다.
    2. 도메인 전문가와 함께 기업의 조직구조와 업무단위를 고려하여 서비스 분리 전략을 정해야 한다.
    3. 서비스 간 통신과 전체 서비스를 관리하기 위한 방안에 대해서 고려해야 한다.
    4. 분산된 서비스 간의 트랜잭션 관리 방안에 대해서 고려해야 한다.
    
    ---
    
    [https://www.samsungsds.com/kr/insights/1239180_4627.html](https://www.samsungsds.com/kr/insights/1239180_4627.html)