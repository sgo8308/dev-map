### 공부해야 하는 이유는?
    
    스프링을 이해하기 위한 기초이자 핵심이다.
    
### 정의는?
    - IOC
        
        IOC는 제어의 역전이라는 뜻이다. 예를 들어 야구 배팅장에서 자동으로 야구공을 쏘는 기계를 생각해보자. 
        이 기계는 아무 생각 없이 들어온 공을 그저 앞으로 내보낼 뿐이다. 
        누군가가 야구공을 넣으면 야구공을 내보낼 것이고 테니스 공을 넣으면 테니스 공을 내보낼 것이다. 
        자신이 직접 공을 선택하고 공을 쏘지 않는다.
        
        즉 제어권이 자기자신이 아닌 다른 무엇에 있는 것이다. 이것을 제어의 역전이라고 한다.
        
        프로그래밍에서 이 야구 기계는 우리가 만든 클래스들을 의미하고 이들을 제어하는 것은 프레임워크다.
        
        이런 역할을 하는 프레임워크로 ‘스프링’ , ‘JUnit’ 등이 있다.
        
    - DI
        
        의존성 주입이라는 뜻이다. 야구공 기계는 어떤 공에 의존할지 모른다. '
        누군가 야구공을 넣으면 야구공에 의존할 것이고 누군가가 테니스 공을 넣으면 테니스 공에 의존할 것이다.
        
        프로그래밍으로 따지면 야구공 기계는 각각의 객체이고 야구공이나 테니스공은 그 객체가 의존하는 객체이고 넣어주는 사람은 프레임워크이다.

### 사용해야 하는 이유는?
    1. 프로그램이 보다 객체 지향적으로 설계되기 때문에 변경이 용이해 진다.

       SRP를 지킬 수 있다.

       → 자신이 의존하는 클래스를 선택하고 생성하는 책임이 의존성을 주입해주는 객체에게로 넘어간다.

       OCP를 지킬 수 있다.

       → 사용 영역과 구성 영역을 나눔으로 인해서 구현체를 변경(기능의 변경, 확장)해도 사용 영역의 코드는 전혀 변경할 필요가 없어진다.

       DIP를 지킬 수 있다.

       → 어떤 클래스가 의존하는 클래스를 직접 생성하면 구체 클래스에 의존할 수 밖에 없다. 의존성을 주입 받음으로써 추상클래스에만 의존할 수 있고 결과적으로 DIP를 지키게 된다.

    2. 의존성을 밖에서 주입해주기 때문에 Mock 객체를 사용해서 유닛 테스트하기가 쉬워진다.
    3. 또한 XML을 이용할 경우 구현체를 바꿀 때 자바 코드 변경, 재컴파일, 재배포할 필요 또한 없어진다.
### 어떤 방식으로 동작할까?

    리플렉션을 통하여 클래스의 이름으로 클래스의 생성자나 메소드 등 클래스의 모든 것을 사용할 수 있다.

    다음은 스프링이 리플렉션을 이용하여 의존관계 주입을 구현할 것이라는 가정 하에 추측한 글이다.

    1. XML Configuration 방식

       리플렉션을 이용하여 등록되어 있는 빈의 클래스 생성자에 접근하여 클래스를 생성한 후 결과값을 반환한다.

    2. @Configuration 어노테이션이 붙어 있는 Configuration 클래스 방식

       Configruation 클래스 안에 있는 빈 생성 메소드들을 통해 빈을 생성하면서 의존 관계를 주입한다.

    3. Component Scan을 이용한 방식 - 생성자 주입

       @ComponantScan이 붙은 Configuration 클래스가 위치한 패키지부터 하위에 있는 모든 패키지(ComponentScan에서 옵션으로 따로 패키지를 지정하지 않을 경우)를 뒤져서 @Componant가 붙은 클래스들을 리플렉션을 이용해서 찾는다.

       찾은 클래스에 대해서 다시 리플렉션을 이용하여 생성자에 접근하여 빈을 생성 후 빈 저장소에 등록한다. 생성자에 @Autowired가 붙어 있다면 인자에 있는 타입을 가진 빈을 빈 저장소에서 찾아서 주입한다.


    ---
    
    [https://better-dev.netlify.app/java/2020/08/27/thejava_11/](https://better-dev.netlify.app/java/2020/08/27/thejava_11/)

### 단점은?
    1. 간단한 프로그램을 만들 땐 번거롭다.
    2. 의존성 주입은 사용 영역과 구성 영역을 분리하기 때문에 런타임에 실제로 어떤 구현체가 실행될지 알기 어렵다.

    → 유지보수가 필요없는 간단한 프로그램을 만드는 경우를 제외하면 장점이 단점을 뛰어넘기 때문에 웬만하면 DI를 사용하는 것이 좋다.


### ADVANCED

### ComponentScan 방식에서 의존관계 주입 방법은 어떤게 있고 언제 어떤 것을 사용하는 것이 좋을까?

    생성자 주입, Setter 주입, 필드 주입, 일반 메서드 주입이 있다.

    1. 생성자 주입

       한 번 설정되면 변경할 수 없고 실행시 필수적으로 등록해줘야 한다.

       보통 프로그램에서 중간에 구현체를 바꿀 일도 없고 바뀌어서도 안되기 때문에 불변 특징을 가진 생성자 주입을 사용하는 것이 좋다.

    2. Setter 주입(수정자 주입)

       중간에 Setter를 통해 의존 객체를 변경 가능하고 실행시 필수적으로 등록해줄 필요가 없다.

       public으로 Setter를 열어놓아야 하기 때문에 누군가가 구현체를 변경할 수 있어서 좋지 않다.

    3. 필드 주입

       코드가 간결해 보이지만 스프링 프레임워크가 없으면 외부에서 변경할 수 가 없다. 
       따라서  순수 자바 코드로 테스트가 불가능하기 때문에 사용하지 않는 것이 좋다.

    4. 일반 메서드 주입

       생성자 주입과 Setter 주입 만으로도 충분하기 때문에 잘 쓰이지는 않는다.

### 순환참조 오류란 무엇이고 어떻게 해결할까?

    이 문제는 닭이 먼저냐 알이 먼저냐라고 할 수 있다. 닭이 나오려면 알이 있어야 되고 알이 나올라면 닭이 있어야 한다.

    비슷한 방식으로 생성자 주입 상황에서 Class A를 생성할 때 B가 필요하고 그래서 B를 생성하려는데 B를 생성할 때 A가 필요 하여 어떤 것도 생성할 수 없는 경우가 있다. 이 경우 스프링은 BeanCurrentlyInCreationException을 낸다.

    해결하기 위해서는 애초에 순환 참조 문제가 생기지 않도록 설계하거나 그럴 수 없을 경우 Setter 주입 방식을 사용한다.
    
    ---

    [https://docs.spring.io/spring-framework/docs/current/reference/html/core.html](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html) - Dependency Resolution Process 파트

### 자동 방식과 수동 방식 중 언제 어떤 것을 사용하는 것이 좋을까?
    - 수동
        - 장점
            - 수동 방식은 사용 영역과 구성 영역을 명확하게 나누어주기 때문에 이상적이다.
            - 클래스 하나만 보면 프로그램에 사용되는 빈이 어떤 것들인지 한 번에 알 수 있어서 문제가 생겼을 때 어디서 문제가 생겼는지 발견하기 좋다.
        - 단점
            - 하지만 매번 클래스 만들 때마다 Configuration 클래스에 등록해주는 것이 귀찮다.
            - 설정 정보가 커지면 설정 정보를 관리하는 것도 부담이 된다.
    - 자동
        - 장점
            - 자동 방식은 편하다. 클래스를 만들고 어노테이션만 붙여주면 된다.
        - 단점
            - OCP가 약간 위배된다. 구현체 변경시 클라이언트 코드에서 어노테이션을 지워주고 새로운 구현체에 붙여줘야 하는 단점이 있다.
            - 현재 사용되는 빈을 찾으려면 클래스를 찾아다니면서 어노테이션을 확인해야 하기 때문에 문제가 생겼을 때 문제를 파악하기가 힘들다.

    **결론**

    1. 자동 방식이 더 실용적이기 때문에 자동 방식을 기본으로 사용한다.
    2. 수동 방식은 AOP나 데이터베이스 연결, 공통 로그 처리를 담당하는 기술 지원 빈에 사용한다.

       왜냐하면 이 빈들은 그 수가 작기 때문에 설정 정보를 관리하기 편하다.

       또 업무 로직 빈들이 문제가 발생했을 때 어디서 발생했는지 파악이 쉬운 반면 기술 지원 빈들은 그게 어렵기 때문에 한 곳에 모아놓는 것이 낫다.
