### PSA(Portable Service Transaction)이란?

기능은 유사하나 사용 방법이 다른 로우 레벨의 다양한 기술에 대해 추상 인터페이스와 일관된 접근 방법을 제공하는 것을 말한다. 

예를 들어 트랜잭션이라는 로우 레벨의 기술을 JPA, JDBC 등의 어떤 구현 기술을 사용해도 일관되게 사용할 수 있도록 PlatformTransactionManager를 제공한다. 

또 커넥션을 얻는다는 로우 레벨의 기술을 DataSource라는 추상화된 인터페이스를 제공해서 일관되게 사용할 수 있게 해준다.

한 편 JavaMail과 같이 테스트를 어렵게 만드는 API에 대해서도 서비스 추상화를 통해 대역을 쉽게 만들고 테스트를 할 수 있게 된다.

PSA를 통해서 비즈니스 로직에 코드들은 로우 레벨의 기술 변화에도 전혀 코드를 고칠 필요가 없게 된다. 만약 수백개의 서비스 클래스가 있다면 PSA가 없을 때는 기술 변화에 따라 수백개의 클래스를 수정함으로 인해 피곤함을 얻고 버그를 삽일할 여지가 있었다면 PSA를 통해 자신감 있게 변화에 대응할 수 있어졌다.

---
토비의 스프링 5장