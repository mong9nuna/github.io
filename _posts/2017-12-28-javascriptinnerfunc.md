---
layout: post
title: '자바스크립트 내장 함수'
author : 효준
date: 2017-12-28 14:15
tags: [javascript]
image: /files/covers/blog.jpg
---

***
### 자바스크립트 내장 함수
***


자바스크립트는 자체적으로 몇가지 함수를 제공합니다. 이렇게 기본으로 내장된 함수를 내장함수라고 부릅니다.
예를들어 지금까지 사용하던 alert() 함수와 prompt() 함수가 내장함수 입니다.
이 절에서는 이외의 다양한 내장함수를 살펴보겠습니다.

***
### 타이머 함수
***

타이머 함수는 특정시간에 특정 함수를 실행할 수 있게 하는 함수입니다.

|메서드 이름|설명|
|:--------|:---|
|setTimeout(function,millisecond)|일정 시간후 함수를 한번 실행합니다.|
|setIntever(function,millisecond)|일정 시간마다 함수를 반복해서 실행합니다.|
|clearTimeout(id)|일정 시간 후 함수를 한번 실행하는것을 중지합니다.|
|clearInterval(id)|일정 시간마다 함수를 반복하는 것을 중단합니다.|


setTimeout() 메서드는 특정한 시간 후에 함수를 한번 실행하고, setInterval() 메서드는
특정한 시간마다 함수를 실행합니다.

```
    setTimeout(function(){ // 3초후 한번실행
        alert('3초가 지났습니다');
    },3000);
    
    setInterval(function(){ // 3초마다 실행
        alert('3초가 지났습니다');
    },3000);
```


두 함수를 종료하기위해선 clearTimeout() 함수와 clearInterval()함수를 사용합니다.
setTimeout()함수와 setInterval()함수를 사용하면 타이머 아이디를 리턴하는데, 이 타이머의 아이디를
clearTimeout()함수와 clearInterval()함수의 매개변수에 넣어주면 타이머를 정지할 수 있습니다.

```
    //1초마다 실행
    var intervalID = setInterval(function() {
        alert('<p>'+new Date()+'</p>');
    },1000);
    
    //10초 후 함수 실행
    setTimeout(function() {
        clearInterval(intervalID);
    },10000);

```


***
### 인코딩과 디코딩 함수
***

|함수 이름|설명|
|:----|:----|
|escape()| 적절한 용도로 인코딩합니다.|
|unescape()|적절한 정도로 디코딩 합니다.|
|encodeURI(uri)|최소한의 문자만 인코딩합니다.|
|decodeURI(encodedURI)|최소한의 문자만 디코딩합니다.|
|encodeURIComponent(uriComponent)|문자 대부분을 모두 인코딩합니다.|
|decodeURIComponent(encodedURI)|문자 대부분을 모두 디코딩합니다.|


세개 모두 비슷한 함수입니다.

> 그런데 '적절한' 과 '최소한'과 '대부분'의 차이는 무엇인가요?

이는 다음과 같이 정리할 수 있습니다.

    escape()
        *영문 알파벳과 숫자,일부 특수문자를 제외하고 모두 인코딩합니다.
        *1바이트 문자는 %XX의 형태로, 2바이트의 문자는 %uXXXX형태로 변환합니다.
    encodeURI()
        *escape()함수에서 인터넷주소에 사용되는 일부특수문자는 변환하지 않습니다.
    encodeURIComponent()
        *알파벳과 숫자를 제외한 모든 문자를 인코딩합니다.
        *UTF-8 인코딩과 같습니다.

현재 가장많이 사용하는 함수는 encodeURIComponent() 입니다.




