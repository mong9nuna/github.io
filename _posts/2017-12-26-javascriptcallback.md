---
layout: post
title: '콜백함수'
author : 효준
date: 2017-12-26 21:14
tags: [javascript]
image: /files/covers/blog.jpg
---

### 콜백 함수

자바스크립트에서는 함수도 하나의 자료형이므로 매개변수로 전달할 수 있습니다.
이렇게 매개변수로 전달하는 함수를 콜백 함수라고 합니다. 다른 프로그래밍 언어에서 찾아보기 힘든 개념이기때문에
처음 접하면 어렵게 느껴질수 있습니다.

다음 callTenTimes() 함수는 함수를 매개변수로 받아 해당함수를 10번 호출하는 함수입니다.

```
function callTenTimes(callback) {
    for(var i = 0 ; i < 10 ; i++) {
        callback();
    }
}

var callback = function () {
    alert('함수호출');
}

callTenTimes(callback);
```

코드를 실행하면 경고창 10번이 출력됩니다.

### 익명콜백함수

위에서 한 코드는 콜백함수를 따로 선언했습니다.
하지만 익명콜백함수 형태도 존재합니다.
사실 익명콜백함수를 사용하는경우가 훨씬 많습니다.

```
function callTenTimes(callback) {
    for(var i = 0 ; i < 10 ; i++) {
        callback();
    }
}

callTenTimes(function() {
    alert('함수 호출');
});
```

### 함수를 리턴하는 함수

이전에 함수를 매개변수로 전달하는것을 살펴보았습니다.
함수를 매개변수로 전달할 수 있다는것은 함수를 리턴하는 함수도 만들수 있다는 뜻이겠죠?

```
function returnFunction() {
    return function() {
        alert('HelloFunction');
    }
}

returnFunction()();
```

returnFunction() 함수를 호출하면 함수가 호출되므로 괄호를 한번더 실행함으로써 리턴된 함수를 실행시킬수 있습니다.

함수를 매개변수로 전달하는 이유는 이벤트부분을 배우면 이렇게 사용하는것이구나 라고 이해할수 있습니다.
하지만 "함수를 리턴하는 함수는 어디에 사용되는 것일까?" 라는 질문의 답은 쉽게 얻기 힘듭니다.
함수를 리턴하는 함수를 사용하는 가장 큰 이유는 클로저 때문입니다.

클로저는 다음포스트에서 뵙겠습니다.