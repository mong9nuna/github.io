---
layout: post
title: '싱글톤 패턴(Singleton Pattern)'
author : 효준
date: 2019-04-30 13:00
tags: [basic]
image: /files/covers/blog.jpg
---

# 시작에 앞서

## 객체란?

+ 객체 : 속성과 기능을 갖춘 것
+ 클래스 : 속성과 기능을 정의한 것
+ 인스턴스 : 속성과 기능을 가진 것 중 실제하는 것

# 학습목표

싱글톤 패턴을 통해서 하나의 인스턴스만 생성하도록 구현 할 수 있다.

# 사전적 의미 Singleton이란?

외동이, 한개의 것, 한장

# Singleton Pattern

하나만 생성해야할 객체를 위한 패턴

# 요구 사항

개발 중인 시스템에서 스피커에 접근 할 수 있는 클래스를 만들어 주세요.

스피커를 여러 클래스에서 접근하면 만약 볼륨1만 올려도 여러클래스를 다 찾아다니며 볼륨을 바꿔줘야한다.

# 소스코드

```java

public class SystemSpeaker {

    static private SystemSpeaker instance;

    private int volume;

    private SystemSpeaker() {
        volume = 5;
    }

    public static SystemSpeaker getInstance() {
        if(instance == null) {
            // 시스템 스피커
            instance = new SystemSpeaker();
        }
        return instance;
    }

    public int getVolume() {
        return volume;
    }

    public void setVolume(int volume) {
        this.volume = volume;
    }
}


-------------------------------

public class Main {
    public static void main(String[] args) {
        SystemSpeaker systemSpeaker1 = SystemSpeaker.getInstance();
        SystemSpeaker systemSpeaker2 = SystemSpeaker.getInstance();

        System.out.println(systemSpeaker1.getVolume()); // 5
        System.out.println(systemSpeaker2.getVolume()); // 5

        systemSpeaker1.setVolume(11);

        System.out.println(systemSpeaker1.getVolume()); // 11
        System.out.println(systemSpeaker2.getVolume()); // 11

        systemSpeaker2.setVolume(22);

        System.out.println(systemSpeaker1.getVolume()); // 22
        System.out.println(systemSpeaker2.getVolume()); // 22

    }
}


```

# 추가사항

인스턴스를 호출 할 때 로그를 찍어주는 소스를 추가

```java
public class SystemSpeaker {

    static private SystemSpeaker instance;

    private int volume;

    private SystemSpeaker() {
        volume = 5;
    }

    public static SystemSpeaker getInstance() {
        if(instance == null) {
            // 시스템 스피커
            instance = new SystemSpeaker();
            System.out.println("새로 생성");
        } else {
            System.out.println("이미 생성");
        }
        return instance;
    }

    public int getVolume() {
        return volume;
    }

    public void setVolume(int volume) {
        this.volume = volume;
    }
}


```