---
layout: post
title: '빌더 패턴(Builder Pattern) - 1'
author : 효준
date: 2019-05-08 13:00
tags: [basic]
image: /files/covers/blog.jpg
---

# 학습목표

<b>복잡한 단계</b>가 필요한 인스턴스 생성을 빌더 패턴을 통해서 구현할 수 있다.

# 사전적 의미의 Builder란?

1. 건축업자, 시공자, 건조자
2. (새 국가 등의) 건설자

# Builder Pattern

복잡한 단계를 거쳐야 생성되는 객체의 구현을 서브 클래스에게 넘겨주는 패턴

# 구현

```java
public class Main {
    public static void main(String[] args) {

        ComputerFactory factory = new ComputerFactory();
        factory.setBlueprint(new LgGramBlueprint()); // Mac이 될수도있고 다양할 수 있음
        factory.make();

        Computer computer = factory.getComputer();

        //Computer computer = new Computer("i7","16g","256g ssd");

        System.out.println(computer.toString());
    }
}


abstract public class BluePrint {
    abstract public void setCpu();
    abstract public void setRam();
    abstract public void setStroage();

    public abstract Computer getComputer();
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

public class ComputerFactory {

    private BluePrint bluePrint;

    public void setBlueprint(BluePrint noteBook) {
        this.bluePrint = noteBook;
    }

    public void make() {
        bluePrint.setCpu();
        bluePrint.setRam();
        bluePrint.setStroage();
    }

    public Computer getComputer() {
        return bluePrint.getComputer();
    }
}


public class LgGramBlueprint extends BluePrint{

    private Computer computer;

    public LgGramBlueprint() {
        computer = new Computer("default","default","default");
    }

    @Override
    public void setCpu() {
        computer.setCpu("i7");
    }

    @Override
    public void setRam() {
        computer.setRam("8g");
    }

    @Override
    public void setStroage() {
        computer.setStorage("256g ssd");
    }

    @Override
    public Computer getComputer() {
        return computer;
    }
}


```

구현 방법은 여러가지가 있다.