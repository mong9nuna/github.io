---
layout: post
title: '추상팩토리 패턴 (Abstract Factory Pattern) - 1'
author : 효준
date: 2019-05-09 13:00
tags: [basic]
image: /files/covers/blog.jpg
---

# 학습목표

관련있는 객체의 생성을 가상화 할 수 있다.

# 소스코드

``` java
import abst.Button;
import abst.GuiFact;
import abst.TextArea;
import concrete.FactoryInstance;

public class Main {
    public static void main(String[] args) {

        GuiFact fact = FactoryInstance.getGuiFact();
        Button button = fact.createButton();
        TextArea textArea = fact.createTextArea();

        button.click();
        System.out.println(textArea.getText());

    }
}

```

# 사용하는 이유
라이브러리 소스 변경 없이 동일한 인터페이스를 제공하면서 어떠한 환경에도 똑같게 작동하도록 만들 수 있다.

이 예제같은경우 각 OS별 GUI를 만들어보았는데 그 FactoryInstance 클래스는 다음과 같다.

``` java
package concrete;

import abst.GuiFact;
import linux.LinuxGuiFact;
import mac.MacGuiFact;
import win.WinGuiFact;

public class FactoryInstance {

    public static GuiFact getGuiFact() {

        switch (getOs()) {
            case 0: return new MacGuiFact();
            case 1: return new LinuxGuiFact();
            case 2: return new WinGuiFact();
        }
        return null;
    }

    private static int getOs() {
        String osName = System.getProperty("os.name");
        if(osName.contains("Mac")) {
            return 0;
        }else if(osName.contains("Linux")) {
            return 1;
        }else if(osName.contains("Window")) {
            return 2;
        }
        return 0; // 원래는 throw 처리 해줘도됨
    }
}

```

위와같은 라이브러리를 받아서 사용한다고하면
각 운영체제별로 신경쓸 필요없이 바로 사용 가능하다.