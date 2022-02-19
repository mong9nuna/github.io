---
layout: post
title: '템플릿 메소드 패턴(Template Method Pattern)'
author : 효준
date: 2019-04-26 13:50
tags: [basic]
image: /files/covers/blog.jpg
---

# 학습목표

일정한 프로세스를 가진 요구사항을 템플릿 메소드 패턴을 이용하여 구현할 수 있다.


# 템플릿 메소드 패턴

템플릿 메소드 패턴이란 <br>

알고리즘의 구조를 메소드에 정의하고 하위 클래스에서 알고리즘 구조의 변경 없이 알고리즘을 재정의 하는 패턴이다. 굉장히 추상적이지만 끝에 다시 정리해보겠다.

## 언제 사용해?

1. 구현하려는 알고리즘이 일정한 프로세스가 있다.
2. 구현하려는 알고리즘이 변경 가능성이 있다.



## 어떻게 사용해?

1. 알고리즘을 여러 단계로 나눈다.
2. 나눠진 알고리즘의 단계를 메소드로 선언한다.
3. 알고리즘을 수행할 템플릿 메소드를 만든다.
4. 하위 클래스에서 나눠진 메소드들을 구현한다.


# 요구사항

+ 신작 게임의 접속을 구현해주세요.
    + requestConnection(String str): String
+ 유저가 게임 접속 시 다음을 고려해야 한다.
    + 보안 과정 : 보안 관련 부분을 처리한다.
        + doSecurity(String string): String
    + 인증 과정 : user name과 password가 일치하는지 확인한다.        
        + authentication(String id, String password) : boolean
    + 권한 과정 : 접속자가 유료 회원인지 무료회원인지 게임 마스터인지 확인한다.
        + authorization(String userName): int
    + 접속 과정 : 접속자에게 커넥션 정보를 넘겨준다.
        + connection(String info) : String


# 코드

```
public abstract class AbstGameConnectHelper {
    protected abstract String doSecurity(String string);
    protected abstract boolean authentication(String id, String password);
    protected abstract int authorization(String userName);
    protected abstract String connection(String info);

    //템플릿 메소드
    public String requestConnection(String encodedInfo) {

        //보안 과정 -> 암호화 된 문자열을 복호화
        String decodedInfo = doSecurity(encodedInfo);
        // 반환된 것을 가지고 아이디, 암호를 할당한다.
        String id = "aaa"; // decodedInfo에서 가져온 것
        String password = "bbb"; // decodedInfo에서 가져온 것

        // 인증 과정
        if(!authentication(id, password)) {
            throw new Error("아이디 암호 불일치");
        }

        String userName = ""; // decodedInfo에서 가져온 것
        // 권한 체크
        int i = authorization(userName);

        switch (i) {
            case 0: // 게임 매니저
                System.out.println("게임 매니저");
                break;
            case 1: // 유료 회원
                System.out.println("유료 회원");
                break;
            case 2: // 무료 회원
                System.out.println("무료 회원");
                break;
            case 3: // 권한 없음
                System.out.println("권한 없음");
                break;
            default: // 기타 상황
                System.out.println("기타 상황");
                break;
        }

        return connection(decodedInfo);
    }
}


public class DefaultGameConnectHelper extends AbstGameConnectHelper {
    @Override
    protected String doSecurity(String string) {
        System.out.println("디코드");
        return string;
    }

    @Override
    protected boolean authentication(String id, String password) {
        System.out.println("DB에서 아이디/암호 확인 과정");
        return true;
    }

    @Override
    protected int authorization(String userName) {
        System.out.println("권한 확인 (게임 매니저)");
        return 0;
    }

    @Override
    protected String connection(String info) {
        System.out.println("마지막 접속 단계");
        return info;
    }
}


public class Main {
    public static void main(String[] args) {
        AbstGameConnectHelper helper = new DefaultGameConnectHelper();

        helper.requestConnection("암호화 된 접속정보");
    }
}

```

위 코드에서 Helper클래스들은 다른 패키지에 두고 실제로는 라이브러리 형태로 배포한다.


# 추가사항

+ 보안 부분이 정부 정책에 의해 강화되었다. 강화된 방식으로 코드를 변경해야 한다.

+ 여가부에서 밤 10시 이후에 접속이 제한 되도록 요구했다.

## 코드

```
package com.method;

public abstract class AbstGameConnectHelper {
    protected abstract String doSecurity(String string);
    protected abstract boolean authentication(String id, String password);
    protected abstract int authorization(String userName);
    protected abstract String connection(String info);

    //템플릿 메소드
    public String requestConnection(String encodedInfo) {

        //보안 과정 -> 암호화 된 문자열을 복호화
        String decodedInfo = doSecurity(encodedInfo);
        // 반환된 것을 가지고 아이디, 암호를 할당한다.
        String id = "aaa"; // decodedInfo에서 가져온 것
        String password = "bbb"; // decodedInfo에서 가져온 것

        // 인증 과정
        if(!authentication(id, password)) {
            throw new Error("아이디 암호 불일치");
        }

        String userName = ""; // decodedInfo에서 가져온 것
        // 권한 체크
        int i = authorization(userName);

        switch (i) {
            case -1: // 나이가 적은유저
                throw new Error("셧다운");

            case 0: // 게임 매니저
                System.out.println("게임 매니저");
                break;
            case 1: // 유료 회원
                System.out.println("유료 회원");
                break;
            case 2: // 무료 회원
                System.out.println("무료 회원");
                break;
            case 3: // 권한 없음
                System.out.println("권한 없음");
                break;
            default: // 기타 상황
                System.out.println("기타 상황");
                break;
        }

        return connection(decodedInfo);
    }
}


package com.method;

public class DefaultGameConnectHelper extends AbstGameConnectHelper {
    @Override
    protected String doSecurity(String string) {
        System.out.println("강화된 알고리즘을 이용한 디코드");
        return string;
    }

    @Override
    protected boolean authentication(String id, String password) {
        System.out.println("DB에서 아이디/암호 확인 과정");
        return true;
    }

    @Override
    protected int authorization(String userName) {
        System.out.println("권한 확인 (게임 매니저)");
        // 서버에서 유저의 나이를 가져와서 확인.. 시간 비교 10시가 지났다면 권한이 없는 것으로 판단.

        return 0;
    }

    @Override
    protected String connection(String info) {
        System.out.println("마지막 접속 단계");
        return info;
    }
}

import com.method.AbstGameConnectHelper;
import com.method.DefaultGameConnectHelper;

public class Main {
    public static void main(String[] args) {
        AbstGameConnectHelper helper = new DefaultGameConnectHelper();

        helper.requestConnection("암호화 된 접속정보");
    }
}


```

