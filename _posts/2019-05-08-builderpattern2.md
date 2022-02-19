---
layout: post
title: '빌더 패턴(Builder Pattern) - 2'
author : 효준
date: 2019-05-08 13:00
tags: [basic]
image: /files/covers/blog.jpg
---

# 학습목표

많은 변수를 가진 객체의 생성을 가독성 높게 코딩 할 수 있다.

# Builder 2

많은 인자를 가진 객체를 생성하는 다른 객체의 도움으로 생성하는 패턴

```java
public class Main {
    public static void main(String[] args) {
        Computer computer = ComputerBuilder
                .start()
                .setCpu("i7")
                .setRam("8g")
                .setStorage("256g ssd")
                .build();

        System.out.println(computer);
    }
}


import java.util.Calendar;

public class ComputerBuilder {

    private Computer computer;

    private ComputerBuilder () {
        computer = new Computer("default", "default", "default");
    }

    public static ComputerBuilder start() {
        return new ComputerBuilder();
    }

    public ComputerBuilder setCpu(String s) {
        computer.setCpu(s);
        return this;
    }

    public ComputerBuilder setRam(String s) {
        computer.setRam(s);
        return this;
    }

    public ComputerBuilder setStorage(String s) {
        computer.setStorage(s);
        return this;
    }

    public Computer build() {
        return this.computer;
    }
}

public class Computer {

    private String cpu;
    private String ram;
    private String storage;

    @Override
    public String toString() {
        return "Computer{" +
                "cpu='" + cpu + '\'' +
                ", ram='" + ram + '\'' +
                ", storage='" + storage + '\'' +
                '}';
    }



    public Computer(String cpu, String ram, String storage) {
        this.cpu = cpu;
        this.ram = ram;
        this.storage = storage;
    }

    public String getCpu() {
        return cpu;
    }

    public void setCpu(String cpu) {
        this.cpu = cpu;
    }

    public String getRam() {
        return ram;
    }

    public void setRam(String ram) {
        this.ram = ram;
    }

    public String getStorage() {
        return storage;
    }

    public void setStorage(String storage) {
        this.storage = storage;
    }

}


```

위와 같은 형태의 빌더패턴이 자주 보던 빌더패턴이다.<br>
하지만 이전글의 빌더패턴1의경우 책에 나온 패턴이다.<br> 템플릿메소드패턴과 유사하다고 생각한다.