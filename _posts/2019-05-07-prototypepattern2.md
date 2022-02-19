---
layout: post
title: '깊은 복사, 얕은 복사'
author : 효준
date: 2019-05-07 14:00
tags: [basic]
image: /files/covers/blog.jpg
---

# 깊은복사, 얕은 복사

```java
public class Main {
    public static void main(String[] args) {
        Cat navi = new Cat();
        navi.setName("navi");

        Cat yo = navi;
        yo.setName("yo");

        System.out.println(navi.getName());
        System.out.println(yo.getName());
    }
}


public class Cat {
    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}


```

위와 같이 할 경우 낮은 수준의 복사(얕은 복사)가 되어 navi와 yo의 주소값이 똑같다.

깊은 복사의 경우(아래의 경우)는 값 자체가 복사되는 것이고 서로 다른 객체를 뜻한다.

```java
public class Main {
    public static void main(String[] args) throws CloneNotSupportedException {
        Cat navi = new Cat();
        navi.setName("navi");

        Cat yo = navi.copy();
        yo.setName("yo");

        System.out.println(navi.getName());
        System.out.println(yo.getName());
    }
}

public class Cat implements Cloneable{
    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Cat copy() throws CloneNotSupportedException {
        Cat ret = (Cat)this.clone();
        return ret;
    }
}

```

Cat 클래스에 copy가 추가되었다.

```java
public class Main {
    public static void main(String[] args) throws CloneNotSupportedException {
        Cat navi = new Cat();
        navi.setName("navi");
        navi.setAge(new Age(2017,3));
        navi.getAge().setYear(2017);
        navi.getAge().setValue(3);

        Cat yo = navi.copy();
        yo.setName("yo");
        yo.getAge().setYear(2018);
        yo.getAge().setValue(2);


        System.out.println(navi.getName());
        System.out.println(yo.getName());

        System.out.println(navi.getAge().getYear());
        System.out.println(yo.getAge().getYear());

        System.out.println(navi.getAge().getValue());
        System.out.println(yo.getAge().getValue());
    }
}

public class Age {

    int year;
    int value;

    public Age(int year, int value) {
        this.year = year;
        this.value = value;
    }

    public int getYear() {
        return year;
    }

    public void setYear(int year) {
        this.year = year;
    }

    public int getValue() {
        return value;
    }

    public void setValue(int value) {
        this.value = value;
    }
}


public class Cat implements Cloneable{
    private String name;
    private Age age;

    public Age getAge() {
        return age;
    }

    public void setAge(Age age) {
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Cat copy() throws CloneNotSupportedException {
        Cat ret = (Cat)this.clone();
        return ret;
    }
}


```

위와 같이 Age클래스로 나이를 나타낼경우엔 생각한대로 결과가 나오지 않게 된다.


```java
public Cat copy() throws CloneNotSupportedException {
        Cat ret = (Cat)this.clone();
        ret.setAge(new Age(this.age.getYear(), this.age.getValue()));
        return ret;
    }
```

위처럼 age도 깊은 복사를 해주어야 원하는 대로 결과가 나오게 된다.