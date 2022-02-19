---
layout: post
title: 'String은 immutable인가?'
author : 효준
date: 2019-05-26 13:00
tags: [java]
image: /files/covers/blog.jpg
---

# String은 immutable인가 ?

우선 이걸 알기 전에 immutable과 mutable에 대해서 알아보자.

# Immutable이란?

Immutable을 사전적으로 찾아보면, 불변의, 변경할 수 없다는 뜻임을 알 수 있다.
사전적 의미에서도 알 수 있듯이 Immutable은 변경이 불가하다. 즉, Immutable 클래스는 변경이 불가능한 클래스이며, 가변적이지 않은 클래스 이다.

만들어진 Immutable Class는 레퍼런스 타입의 객체이기 때문에 heap영역에 생성이 된다.<br>

자바에서 이런 Immutable 클래스로 어떤 것들이 있을까?

제목으로 말한 String 뿐 아니라 Boolean, Integer, Float, Long 등등이 있다. 이러한 클래스들은 heap영역에서 변경 불가능한 것이지<br>
재 할당을 못하는 건 아니다. 즉, 

``` java
   String a = "aa";
   a = "bb";
```

이런식으로 재할당이 가능하다. a가 참조하고 있는 heap 영역의 객체가 바뀌는 것이지 heap 영역에 있는 값이 바뀌는게 아니다.

좀 더 풀어보면 a가 처음 참조하고 있는 "aa"값이 "bb"로 변경되는 것이 아니라 아예 "bb"라는 새로운 객체를 만들고 그 객체를 a가 참조하게 되는 것이다.
이렇게 했을 경우 "aa"값을 지니고 있는 객체는 이제 그 누구도 참조하고 있지 않는 객체가 되며 gc대상이 된다.