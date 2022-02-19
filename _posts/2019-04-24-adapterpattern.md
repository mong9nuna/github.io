---
layout: post
title: '아답터 패턴 Adapter Pattern'
author : 효준
date: 2019-04-24 13:50
tags: [basic]
image: /files/covers/blog.jpg
---

# Adapter Pattern

## Adapter 란?
사전적 의미로는 기계.기구 등을 다목적으로 사용하기 위한 부가 기구이다.

220v 코드를 110v로 바꾸기위해 어댑터가 필요하다.

## 요구사항

+ 두 수에 대한 다음 연산을 수행하는 객체를 만들어 주세요.
    + 수의 두배의 수를 반환 : twiceOf(Float) : Float
    + 수의 반(1/2)의 수를 반환 : halfOf(Float) : Float

+ 구현 객체의 이름은 Adapter로 해주세요.
+ Math 클래스에서 두 배와 절반을 구하는 함수는 이미 구현 되어 있습니다.


## 알고리즘의 변경

+ Math 클래스에 새롭게 두배를 구할수 있는 함수가 추가되었습니다.
새로 구현된 알고리즘을 이용하도록 프로그램을 수정해주세요.
+ 절반을 구하는 기능에서 로그를 찍는 기능을 추가해 주세요.

```
public class Math {

    public static double twoTime(double num) {
        return num * 2;
    }
    public static double half(double num) {
        return num/2;
    }
    // 강화된 알고리즘
    public static Double doubled(Double d) {
        return d * 2;
    }
}


public interface Adapter {
    // 원하는 기능
    public Float twiceOf(Float f);
    public Float halfOf(Float f);
}


public class AdapterImpl implements Adapter {

    @Override
    public Float twiceOf(Float f) {
        Float float1 = Math.doubled(f.doubleValue()).floatValue();
        return float1;
    }

    @Override
    public Float halfOf(Float f) {
        System.out.println("절반 함수 호출");
        return (float) Math.half(f.doubleValue());
    }
}


```


미리 구현된 기능 Ex.공통함수 등을 수정할 수 없을 때, 즉 구현된 기능의 약간의 수정이 필요할 때 사용하면 좋을듯 하다.