### Netty란 무엇이고 왜 사용할까?
    
    Java의 I/O 방식에는 Blocking I/O 방식과 Nonblocking I/O 방식이 있다. 
    
    BIO는 한 네트워크 연결 당 하나의 쓰레드가 필요하다. 따라서 네트워크 연결이 많아지면 똑같은 수만큼의 쓰레드가 필요하다. 
    
    쓰레드는 OS에 따라 약 64 KB 에서 1 MB 스택공간이 필요한데 네트워크를 통해 인풋을 기다리는 쓰레드는 블록된 상태로 메모리 공간만 차지하고 아무것도 하지 않게 된다. 
    
    또한 많은 쓰레드는 컨텍스트 스위칭을 자주 유발시키고 이는 성능 저하로 이어진다.
    
    즉 Blocking I/O를 이용한 네트워크 통신 방식은 메모리과 성능면에서 좋지 않다.
    
    NIO는 여러개의 네트워크 연결을 쓰레드 하나로 처리할 수 있다. 
    이는 Selector라는 것을 쓰기 때문에 가능한데, Selector는 이벤트 통지 방식의 API로써 논블로킹 소켓들을 주시하고 있다가 데이터를 읽거나 쓸 수 있는 상태가 되면 쓰레드에게 통지해준다.
    
    쓰레드는 그 동안 다른 일을 할 수도 있고 통지를 받게 되면 그에 따른 네트워크 작업을 해주면 된다.
    
    즉 NIO 방식을 쓰면 적은 쓰레드로 많은 네트워크 작업을 할 수 있게 되고 BIO가 갖는 단점을 상쇄할 수 있다.
    
    하지만 NIO를 이용해서 높은 트래픽에 상황에서도 안정적이고 효율적인 네트워크 애플리케이션을 만들기 위해서는 다루기 힘들고 에러가 발생하기 쉬운 작업이다. 
    
    Netty는 NIO를 사용해서 네트워킹 프로그램을 만드는 작업을 쉽게 해줄 수 있게 해주는 프레임워크이다.
    
    ---
    
    [https://livebook.manning.com/book/netty-in-action/chapter-1/46](https://livebook.manning.com/book/netty-in-action/chapter-1/46) 네티 인 액션 1장