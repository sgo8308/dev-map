### amortized time complexity란?
    분할 상환 시간 복잡도라는 뜻으로 개별 연산의 시간 복잡도를 파악하는 것이 아니라, 
    연속적인 연산 여러개를 진행하고 평균을 구해서 시간복잡도를 파악한 것을 말한다.

    예를 들어 Dynamic Array의 경우 맨 뒤에 요소를 추가할 때 상수 시간이 걸린다. 
    그러나 배열이 꽉찰 때마다 사이즈를 늘려주어야 하기 때문에 n의 시간이 걸리게 된다.

    이 때 처음부터 배열이 다 찬 후 1개를 더 추가하기까지를 묶어서 판단하면 amortized 시간복잡도를 구하는 방법을 다음과 같다.

    일반적인 시간복잡도를 사용해서 최악의 시간복잡도를 표시한다면 O(N)일 것이다. 
    그러나 여러 연산(상수 연산 여러개 N 연산 1개 이런 식)들을 사용해서 이것의 평균적인 시간을 구한다면
    상수 연산들이 N 연산들을 부담하게 되기 때문에 O(1)이 되게 된다.

    이 때는 amortized O(1)이라고 표기하게 된다.
