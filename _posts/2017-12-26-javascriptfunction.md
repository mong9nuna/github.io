---
layout: post
title: '자바스크립트에서의 함수'
author : 효준
date: 2017-12-26 20:58
tags: [javascript]
image: /files/covers/blog.jpg
---

### 자바스크립트에서의 함수

### 익명함수

```
var 함수 = function() {};
```

### 선언적 함수

```
function 함수() {

}
```

이둘은 조금의 차이는 있지만 비슷한 기능이다.

```
<script>
    function 함수() {alert("함수 A");}
    function 함수() {alert("함수 B");}
    함수();
</script>
```
위 코드에서 무엇이실행될까? 예상해보자.
당연히 뒤에있는 함수 B가 화면에 뜰것이다.



```
<script>
    var 함수 = function() {alert("함수 A")}
    var 함수 = function() {alert("함수 B")}
    함수();
</script>
```

그렇다면 이건 어떨까?
위 코드역시 함수 B가 호출될것이다.


```
<script>
    함수();
    var 함수 = function() {alert("함수 A")}
    var 함수 = function() {alert("함수 B")}
</script>
```

위 코드는 어떻게 될까?
찍어본다면 아무것도 출력되지 않을것이다.
함수라는 익명함수가 아직 생성되지 않았기 때문이다.
그래서 함수(); 라는 코드가 무엇을 실행해야하는지 모르기때문에 에러가 난다.

```
<script>
    함수();
    function 함수() {alert("함수 A");}
    function 함수() {alert("함수 B");}
</script>
```

위코드처럼 선언적함수로 바꿔서 하면 어떻게 될까?
정상적으로 실행된다. <b>웹 브라우저는 script태그 내부의 내용을 한 줄씩 읽기 전에 선언적 함수부터 읽습니다.</b>
따라서 위코드는 2->3->1 번째줄 순으로 실행됩니다.

```
<script>
    var 함수 = function() { alert("함수A")}
    function 함수() {alert("함수B")}
    
    함수();
</script>
```

이렇게되면 어떨까?
헷갈릴수 있지만 조금 생각해보면 선언적함수부터 읽기때문에 익명함수인 var 함수에 의해 덮어 씌어진다.
따라서 함수 A가 출력된다.

### 가변 인자 함수

가변 인자 함수는 매개변수의 개수가 변할 수 있는 함수입니다.
자바스크립트는 매개변수의 개수를 정의된 것과 다르게 사용해도 괜찮지만, 여기서 말하는 가변 인자 함수는 매개변수를 선언된 형태와 다르게 사용했을 때,
매개변수를 모두 활용하는 함수를 뜻합니다.

```
<script>
    function sumAll(){
        var output = 0,
            i;
        for(i = 0 ; i < arguments.length; i++) {
            output += arguments[i];
        }
        return output;
    }
    
    alert(sumAll(1,2,3,4,5,6,7,8,9));
    
</script>
```
위 코드를 실행하면 45가 나오는것을 알 수 있다.
이처럼 매개변수가 들어가면 arguments배열에 들어가게된다.

### 내부 함수

내부함수는 프로그램규모가 커질수록 많은 사람과 개발을 하게되며 그 와중에 충돌이 생길 수 있는데 그걸 막는 방법입니다.
내부함수는 다음처럼 함수 내부에 선언하는 함수를 의미합니다.

```
function outter() { // 외부함수
    function inner1() { // 내부함수 1
    
    }
    function inner2() { // 내부함수 2
    
    }
    ~~~
}

```

위 코드처럼 선언합니다.

피타고라스의 정리를 구현하는 함수를 만들면서 공부해보겠습니다.
이 함수는 밑변과 높이를 매개변수로 받아 빗변의 길이를 리턴합니다.

```
function pythagoras(width,height) {
    return Math.sqrt(width*width + height*height);
}

```

숫자를 제곱하는 부분은 자주 사용하는 내용이므로 별도의 함수 square()로 만들겠습니다.

```
function square(x) { // 제곱 구하는 함수
    return x*x
}

function pythagoras(width,height) { // 피타고라스 함수
    return Math.sqrt(width*width + height*height);
}
```

현재코드자체는 아무문제 없습니다. 하지만 다른 팀원이 square를 직각으로 오해해서 직각삼각형인지 확인하는 함수를 만들었다면 문제가 발생합니다.

```
function square(x) { // 최씨가 만든 함수
    return x*x;
}

function pythagoras(width,height) { // 피타고라스 함수
    return Math.sqrt(width*width + height*height);
}

alert(pythagoras(3,4));

//동료가 모르고 만든 함수
//삼각형이 직각인지 확인하는 함수
function square(width,height,hypotenuse) {
    if(width*width + height*height == hypotenuse * hypotenuse) {
        return true;
    }else {
        return false;
    }
}

```

위 코드처럼 동료가 만든함수의 이름과 내가 미리 만든 함수의 이름이 같다면 동료의 함수에 덮어 씌어집니다.
따라서 기존 square함수를 지키고 싶다면 pythagoras함수 내부에서만 사용할 수 있게 해야합니다.

```
function pythagoras(width,height) { // 피타고라스 함수
    function square(x) {
        return x*x;
    }
    
  return Math.sqrt(square(width) + square(height));
}
```

이렇게 한다면 pythagoras 외부에서는 square함수를 사용할 수 없습니다.

### 자기 호출 함수

다른 개발자에게 영향을 주지 않게 다음코드처럼 함수를 만들어 사용하는 경우가 있습니다.

```
<script>
    (function() {
        //코드
    })();

</script>

```

위 코드처럼 생성하자마자 호출하는것을 자기호출함수라고 합니다.