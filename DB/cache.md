### DB 데이터 캐싱하는 방법은 어떤게 있을까?
1. 로컬캐시
    
    자주 사용되는 데이터를 가져온 후 서버 메모리에 HashMap과 같은 형태로 저장한다. 또는 디스크에 저장할 수도 있다. 이 후 동일한 요청이 들어오면 이 요청을 key로 삼아 HashMap에서 가져와서 전달한다.
    
    - 장점
        - 빠르다.
    - 단점
        - 서버가 분산될 경우 캐시미스 발생이 많아진다.
            
            11캐시는 최근에 요청받은 데이터는 다시 요청할 가능성이 높다는 지역성의 원리에 기반한다. 그런데 예를 들어 A서버에 요청을 해서 데이터를 캐싱해놓았는데 같은 요청이 이번엔 B서버로 요청이 가게 되면 캐시 미스가 발생한다. 
            
        - 동일한 데이터를 여러 곳에서 보관할 수 있기 때문에 메모리 효율적이지 않다.
2. 전역캐시
    
    Redis와 같은 인메모리 DB를 사용하여 자주 사용하는 데이터를 캐싱한다. 여러 서버들은 모두 이 저장소를 이용한다.
    
    캐시 미스가 발생할 경우 2가지 방법으로 해결할 수 있다.
    
    1. Redis가 직접 DB에 요청하여 데이터를 가져오고 캐시를 업데이트한다. 
    2. 각 서버가 DB에 요청하여 데이터를 가져온다. 이 후 캐싱이 필요한 데이터를 캐시 저장소에 업데이트한다.
    
    2번 방식은 여러 서버가 동일한 데이터를 요청하는 것과 같은 비효율이 발생할 수 있어서 1번 방식이 많이 쓰인다. 한 편 1번 방식이 좋을 때도 있다. 
    
    - 장점
        - 여러 서버 간 데이터 공유가 쉽다.
    - 단점
        - 요청 갯수가 급격히 증가하면 하나의 캐시 서버로는 감당하기 힘들어져 추가적인 작업이 필요하게 되고 이는 복잡해진다는 것을 의미한다.
3. 분산캐시
    
    여러 서버들이 각각 서로 다른 캐시들을 보관한다. 자신의 캐시를 보고 필요한 데이터가 없다면 DB에 접근하기 전에 다른 서버에 캐시를 확인한다. Consistent Hashing 함수를 사용해 분산 캐시내에 어디에 데이터가 보관되었는지 빠르게 찾을 수 있다.
    
    - 장점
        - 새로운 서버가 추가되는 것이 그대로 전체 캐시 크기의 향상을 가져온다.
    - 단점
        - 하나의 서버에 장애가 발생하면 그대로 캐시를 잃어버리거나 처리할 방법이 필요한데, 꽤 복잡할 수 있다.
        - 서버를 추가하거나 사라지면 알고리즘이 꼬이게 된다. 컨시스턴트 해싱 으로 인한 문제. 앞에 로케이터 시스템을 두면 해결가능

---

[https://d2.naver.com/helloworld/206816#:~:text=대해서 알아보려 한다.-,캐시,-캐시는 최근에 요청받은](https://d2.naver.com/helloworld/206816#:~:text=%EB%8C%80%ED%95%B4%EC%84%9C%20%EC%95%8C%EC%95%84%EB%B3%B4%EB%A0%A4%20%ED%95%9C%EB%8B%A4.-,%EC%BA%90%EC%8B%9C,-%EC%BA%90%EC%8B%9C%EB%8A%94%20%EC%B5%9C%EA%B7%BC%EC%97%90%20%EC%9A%94%EC%B2%AD%EB%B0%9B%EC%9D%80)
    
### 어느 경우에 데이터 정합성이 깨질 수 있을까?
    
서버에서 데이터가 업데이트 되었는데 캐시는 업데이트가 되지 않았을 때
    
### 정합성을 맞추기 위해서는 어떻게 하면 좋을까?
    
캐싱 전략에 따라 다르다.

1. Lazy Loading 전략
    
    데이터가 요청 받을 때 캐시에 없다면 그 때 DB에 질의하여 데이터를 갖고온 후 캐시에 업데이트한다. 이 경우 캐시에 데이터가 존재하는 한 정합성이 어긋난 데이터를 업데이트할 수 없다.
    
    - 장점
        
        요청받은 데이터만 캐시에 보관하기 때문에 효율적이다.
        
    - 단점
        
        초기에 캐시를 적용하지 않을 때보다 더 성능저하가 있을 수 있다. 
        
        캐시에 접근 → 캐시 미스→ 다시 DB에 접근 → 캐시 업데이트 
        
        와 같이 추가 과정이 생기기 때문이다.
        
2. Write-through 전략
    
    DB에 데이터가 추가되거나 업데이트 될 때만 캐시에 데이터를 추가하거나 업데이트한다.
    
    여러 서버를 사용하는 경우 업데이트될 때마다 이벤트를 보내고 이 이벤트가 올 때마다 캐시를 업데이트 해준다. 또는 서버를 한 대만 사용할 경우 DB에 update하자마자 캐시도 업데이트한다.
    
    - 장점
        - 절대 캐시 데이터가 stale될 일이 없다.
    - 단점
        - 데이터가 업데이트되거나 추가되지 않은 데이터는 캐시에 추가되지 않기 때문에 자주 읽히는 데이터가 계속해서 캐시에 없을 수 있다.
        - 공간 낭비
            
            한 번만 읽힌 데이터가 계속해서 캐시 안에 존재할 수 있다. 
            
        - wirte 성능 저하
            
            wirte작업을 할 때 캐시도 같이 업데이트 해야하기 때문에 wirte 작업이 느려진다.
            
3. TTL 전략
    
    DB데이 데이터를 쓸 때 또는 업데이트할 때 캐시에도 데이터를 쓰거나 업데이트하는데 이 때 유효시간을 준다. 또는 데이터 요청이 들어왔을 때 캐시를 확인하고 없다면 DB에 요청하는데 이 후 캐시에 쓸 때 유효시간을 준다.
    
    유효시간이 지난 캐시는 DB에서 새로 가져온다. 유효시간이 지나기 전까지는 DB 업데이트된 데이터에 대해 정합성이 어긋날 수 있지만 그래도 최대한 fresh하게 캐시를 유지한다.
    

일반적으로는 이 전략들을 섞어서 서로의 단점을 보완한다.

---

[https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/Strategies.html#Strategies.LazyLoading](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/Strategies.html#Strategies.LazyLoading)

[https://docs.aws.amazon.com/ko_kr/whitepapers/latest/database-caching-strategies-using-redis/caching-patterns.html](https://docs.aws.amazon.com/ko_kr/whitepapers/latest/database-caching-strategies-using-redis/caching-patterns.html)

### 캐시 업데이트에서 push 모델, pull 모델은 무엇이고 각각의 장단점은 무엇일까?
- push 모델
    
    데이터를 갖고 있는 쪽에서 데이터 변경이 발생하면 변경에 대한 이벤트를 이벤트 채널로 보내는 것을 말한다.
    
- pull 모델
    
    이벤트를 소비하는 쪽에서(캐시 업데이트 해야 하는 곳) 수시로 데이터에 변화가 생겼는지 확인하는 것을 말한다.