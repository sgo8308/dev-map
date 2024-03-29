<details>
<summary>인터럽트란 무엇일까?</summary>
<br>

CPU가 프로그램을 실행하고 있을 때, 입출력 하드웨어 장치 또는 예외상황이 발생하여 처리가 필요할 경우에 CPU에 알려서 처리하는 기술
</details> 

<details>
<summary>인터럽트는 왜 필요할까?</summary>
<br>

선점형 스케쥴러, IO Device와의 커뮤니케이션, 에러 상황 핸들링 등을 위해서 인터럽트가 필요하다.
</details>

<details>
<summary>인터럽트의 종류는 어떤게 있을까?</summary>
<br>

- 내부 인터럽트(소프트웨어 인터럽트)
    
    주로 프로그램 내부에서 잘못된 명령이나 문제가 생겼을 때 발생합니다.
    
    Ex) 0으로 나누었을 때, 
    
    사용자 모드에서 허용되지 않은 명령 또는 공간 접근시
    (포인터가 커널 영역의 메모리 주소를 가르키거나 등등..)
    

- 외부 인터럽트(하드웨어 인터럽트)
    
    전원 이상, 기계 문제, 키보드 등 IO 이벤트, Timer 이벤트(일정 시간마다 CPU에 인터럽트를 검. 선점형 스케쥴러를 위해 필요함)등이 있습니다.
</details>

<details>
<summary>인터럽트는 내부적으로 어떻게 동작할까?</summary>
<br>

인터럽트가 발생하면 CPU는 사용자 모드에서 커널 모드로 바뀐다.

이 후  CPU는 커널 영역에 기록되어 있는 IDT(Interrupt Descriptor Table)로 향한다.

IDT에는 각각의 인터럽트의 번호와 해당하는 인터럽트를 처리하는 함수의 주소가 매핑되어 있다. 

CPU는 들어온 인터럽트 번호에 해당하는 함수를 실행하고 다시 사용자 모드로 복귀한다.

시스템 콜도 소프트웨어 인터럽트에 일종이며 내부 인터럽트는 따지고 보면 운영체제가 인터럽트를 일으키고 다시 자기 자신 안에 있는 인터럽트 처리 함수를 호출하는 셈이다.

---

제로베이스, 컴퓨터 공학 전공자 따라잡기, Chapter3 프로세스와 스케쥴러의 이해 09. 인터럽트 내부 동작
</details>
            