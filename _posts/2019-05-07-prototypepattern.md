---
layout: post
title: '프로토타입 패턴(Prototype Pattern)'
author : 효준
date: 2019-05-07 13:00
tags: [basic]
image: /files/covers/blog.jpg
---

# 학습 목표

+ 프로토타입 패턴을 통해서 복잡한 인스턴스를 복사 할 수 있다.

# 사전적 의미의 프로토 타입 이란?

+ 원형; 본, 표준, 모범.
+ (어떤 것의)옛날의 유사물.
+ 생 원형(archetype)

# Prototype Pattern

### 생산 비용이 높은 인스턴스를 복사를 통해서 쉽게 생성할 수 있도록 하는 패턴

# 인스턴스 생산 비용이 높은 경우

+ 종류가 너무 많아서 클래스로 정리되지 않는 경우
+ 클래스로부터 인스턴스 생성이 어려운 경우

# 요구사항

+ 일러스트레이터와 같은 그림 그리기 툴을 개발 중입니다. 어떤 모양을 그릴 수 있도록 하고 복사 붙여넣기 기능을 구현해주세요.
+ (hint) 자바에서는 이미 Cloneable 이란 인터페이스가 존재한다.

# 더 공부해봅시다.

+ 복사후 붙여넣기 하면 옆으로 살짝 옆으로 이동해주세요.

```java
public class Main {

    public static void main(String[] args) throws CloneNotSupportedException {
        Circle circle1 = new Circle(1,1,3);
        Circle circle2 = circle1.copy();

        System.out.println(circle1.getX()+ " , " + circle1.getY() + ", " + circle1.getR());

        System.out.println(circle2.getX()+ " , " + circle2.getY() + ", " + circle2.getR());

    }
}

public class Shape implements Cloneable {

    private String id;

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }
}



public class Circle extends Shape{
    private int x,y,r;

    public Circle(int x, int y, int r) {
        this.x = x;
        this.y = y;
        this.r = r;
    }

    public Circle copy() throws CloneNotSupportedException{
        Circle circle = (Circle) clone();
        circle.x = x + 1;
        circle.y = y + 1;

        return circle;
    }

    public int getX() {
        return x;
    }

    public void setX(int x) {
        this.x = x;
    }

    public int getY() {
        return y;
    }

    public void setY(int y) {
        this.y = y;
    }

    public int getR() {
        return r;
    }

    public void setR(int r) {
        this.r = r;
    }
}



```



# 정리

카피 할 수 있도록 Cloneable을 상속받아 해당 클래스는 항상 복사가 가능한 상태로 만들 수 있다.