---
layout: post
title: '스프링 전략패턴, IOC'
author : 효준
date: 2019-04-25 10:20
tags: [spring]
image: /files/covers/blog.jpg
---

# DI

스프링의 모든 기술의 근간이 되는, 스프링의 핵심 엔진인 DI에 대해서 알아보기 전에 전략 패턴과 IOC에 대한 개념부터 알고가자.

전략패턴은 이전 포스팅에서와 같이 디자인패턴중 하나인데, 간단하게 상황에 따라 전략을 바꿔가며 사용할 수 있다고해서 전략패턴이라 한다.

# 전략 패턴

우리가 자주 사용하는 DB커넥션을 얻는 과정을 전략패턴화 해볼텐데, 사전에 몇가지 작업을 하고 진행할 것이다.
먼저 일반적으로 DB커넥션을 얻어오는 코드는 아래와 같다.

```
public List selectData(){
    Class.forName("com.mysql.jdbc.Driver");
    Connection conn = DriverManager.getConnection(""
            + "jdbc:mysql://localhost:3306/testDB","id","pwd");
 
     // Connection을 통한 쿼리 작업
}
```

지금 위에 메서드는 문제점이 있다. 현재는 그냥 하나의 메서드라 별 문제가 없어 보이지만 규모가 큰 프로젝트의 경우 위와 같은 메서드가 한두개가 아닐 것이다. select외에 insert, update,delete등 아주 많을 것이다.

그러한 상황에서 메서드마다 저런식으로 Connection을 얻는 코드를 입력한다면 상당한 코드의 중복이 일어나게 된다.
100개의 메서드가 있으면, 위의 커넥션을 얻는 코드가 100번 들어가게 된다.

이상황에서 만약 DB접속정보가 바뀐다면??

아주 끔찍하다. 바로 메서드의 수만큼 다 변경해줘야 하기 때문이다.
이 과정에서 실수할 가능성이 매우 높기 때문에 굉장히 위험하기도 하다.

위의 문제점을 해결해 줄 수 있는 방안으로 메서드 추출이 있는데, 중복이 되는 Connection을 얻어오는 과정을, 하나의 메서드로 추출하는 방법이다.

```
public class CarDAO{
    public List selectSUVData() throws Exception{
        Connection conn = getConnection();
         
        List list = // SUV 정보 쿼리
        return list;
    }
  
    public List selectSedanData() throws Exception{
        Connection conn = getConnection();
         
        List list = // 세단 정보 쿼리
        return list;
    }
  
    public List selectCoupeData() throws Exception{
        Connection conn = getConnection();
         
        List list = // 쿠페 정보 쿼리
        return list;
    }
 
    public Connection getConnection(){
        Class.forName("com.mysql.jdbc.Driver");
        Connection conn = DriverManager.getConnection(""
                + "jdbc:mysql://localhost/testDB","id","pwd");
        return conn;
    }
}
```

이런식으로 말이다.
이렇게 하면 아무리 쿼리 작업을 하는 메서드가 많더라도, getConnection() 메서드의 코드만 수정해주면 된다.

중복을 제거하긴 했지만, 위 방식은 그렇게 이상적인 방식은 아니다.
OOP의 개념중에 <b>관심사의 분리</b>라는것이 있다.
관심이 같은 것 끼리는 모으고, 관심이 다른 것은 떨어져 있게 하는 것이다.

위의 상황에서 살펴보면, 자동차 리스트를 얻어오는 행위와 DB커넥션을 맺는 행위는 엄연히 다른 관심사이다.

이러한 상황에서는 따로 클래스를 분리해줘서 관심사를 분리시키는 것이 좋다.

당장은 클래스를 굳이 하나 더 만들어야 해서 소스가 많아지고 번거로운 것 같지만, 조금만 규모가 커져도 차이점을 확 느낄 수 있을 것이다.

언제나 상황에 따라 적절하게 관심사를 분리해줘야 유지보수가 편하고, 나중에 확장과 변경에도 용이해진다.

Connection을 맺는 객체는 아래와 같은 형태로 바뀌게 될 것이다.

```
public class CarDAO{
    private DBConnection dbConnection = new DBConnection();
 
    public List selectSUVData(){
        Connection conn = dbConnection.getConnection();
        // 자동차 정보 쿼리
        return list;
    }
 
    ...
 
}
 
public class DBConnection{
    public Conneciton getConnection(){
        Class.forName("com.mysql.jdbc.Driver");
        Connection conn = DriverManager.getConnection(""
                + "jdbc:mysql://localhost/testDB","id","pwd");
        return conn;
    }
}

```

확실히 관심사가 분리되었다.
이제 전략패턴을 적용하기 위한 사전단계까지 완료되었다.

조금 더 여러가지 상황에서 생각해보자.
현재 DBConnection 클래스의 getConnection메서드에는 MySql에 대한 Connection을 얻는 코드가 있다.

근데 만약 DB의 종류나 URL, 계정 정보가 자주 바뀐다면 어떻게 될까? (DB를 변경해가며 테스트하는 상황 등)

뭐 간단하게 매번 DBConnection 클래스의 getConnection 메서드의 내용을 수정해주면 되긴 한다. 하지만 매번 그러기엔 너무 비효율적이다.

그래서 그냥 각각 클래스로 만들어 보았다.

MySqlDBConnection, OracleDBConnection, OracleDBConnection_dev 등의 형태로 말이다.

근데 막상 만들고 보니 사용하는 것이 문제이다. 각자 클래스들로 구분해놓으면 변경 될 때마다 해당 클래스의 선언부를 다 변경해줘야 하기 때문이다.

```
DBConnection dbConnection = new DBConnection() 

↓↓변경↓↓

MySqlDBConnection dbConnection = new MySqlDBConnection; // MySql 사용시

OracleDBConnection dbConnection = new OracleDBConnection; // Oracle 사용시

```

위와 같이 매번 상황에 따라 변경되어야 하는 꼴이 되는 것이다.

이는 DB의 정보가 변경될 때마다 DAO의 소스를 다 바꿔줘야 하므로, 매우 비효율 적이다.

하지만 자바에는 이를 해결할 아주 좋은 문법이 존재한다. 바로 <b>인터페이스</b>이다.

<b>DBConnection이라는 인터페이스를 정의하고, 나머지 MySQLDBConnection, OracleDBConnection 등의 클래스를 DBConnection 인터페이스를 상속한 클래스로 만들어 버리면 된다.

그리고 DBConnection 구현체는 DAO코드 내에서 직접 생성하지 않고, 생성자나 파라미터를 통해 전달받게 해버리면 변경에 매우 유연한 코드가 된다.
</b>

즉 아래와 같이 변경될 것이다.

```
public class CarDAO{
    private DBConnection dbConnection;
 
    public setDbConnection(DBConnection dbConnection){
        this.dbConnection = dbConnection;
   }
         
    public List selectSUVData(){
        Connection conn = dbConnection.getConnection();
        // 자동차 정보 쿼리
        return list;
    }
}
```

이렇게 해주면 사용하는 DB의 정보가 변경되더라도 DAO는 항상 변함없이 유지할 수 있게 된다.

위의 방식이 <b>전략패턴</b>이다.

DBConnection 인터페이스를 변수로 선언했기 때문에, 저 변수에는  MySQLDBConnection, OracleDBConnection 등 DBConnection 인터페이스를 구현한 클래스라면 어느 클래스가 와도 상관이 없게 된다.

즉, 상황에 따라 구현 클래스를 바꿔 끼워가며(전략을 바꿔가며) 사용할 수 있다는 점이다.
진짜 아주 좋은 디자인 패턴인 것 같다.

# IOC

그럼 이제 IOC에 대해 알아보자.

우리가 앞서 전략패턴을 적용하면서, 새로운 대상을 하나 등장시켰다.

> 그리고 DBConnection 구현체는 DAO코드 내에서 직접 생성하지 않고, 생성자나 파라미터를 통해 전달받게 해버리면 변경에 매우 유연한 코드가 된다.

라고 말했다. 생성자나 메서드의 파라미터를 통해 전달받게 한다고...

그럼 전달은 누가해줄까?

여기서 IOC의 개념이 등장한다.

```
IOC란 Inversion Of Control의 약자로, 제어의 역전이라는 뜻을 가지고 있다.

일반적인 프로그램은 자신이 사용할 오브젝트를 직접 선택하고, 생성한다.
변경 전의 DAO에서 보았듯이 DAO는 자신이 사용할 Connection 클래스를 직접 선택하고, 생성했다. ex) new MySQLDBConnection();

오브젝트의 대한 제어권을 자신이 가지고 있는, 능동적인 상태인 것이다.
하지만 마지막에 전략패턴을 적용시키고, 변화에 유연한 코드를 만들면서 우리는  DAO를 수동적인 상태로 변환시켰다.
이때까진 필요한 오브젝트를 직접 만들다가, 이젠 남이 만들어 준걸 전달받게 된다.
오브젝트에 대한 제어가 역전된 것이다.

제어의 역전이란 말 그대로 제어권을 역전 시키는 것으로써,

제 3자에게 오브젝트에 대한 제어권을 넘겨주고, 자신은 제 3자가 선택하고 생성한 오브젝트를 받아서 사용하는 수동적인 상태가 되는 것을 말한다.

이는 스프링 프레임워크가 아닌 다른곳에도 이미 폭 넓게 적용되어 있는 개념이다.
```

아까 전달을 누가해주지? 라는 의문을 가졌었다.
IOC의 개념을 알고나면 답이 나오게 된다. 제 3자에게 전달을 받는 것이다.
new MySQLDBConnection(); 과 같은 코드를 이제 제 3자가 처리해줘야 한다.

제 3자라고 별 다를 것 없다. 그냥 클래스로 만들자. 이름은 DaoFactory정도로 할 것이다.

```
public class DaoFactory{
    public CarDAO carDAO(){
        CarDAO carDao = new CarDAO();
        carDao.setDbConnection(getConnection());
         
        return carDao;
    }
     
    public DBConnection getConnection(){
        return new MySqlDBConnection_real(); // 변경되는 부분
    }
}
```

제 3자가 사용할 클래스를 선택하고, 전달해준 뒤, 반환해주고 있다.
이로 인해 DB정보가 변경되더라도 변경될 곳은 제 3자인 DaoFactory로 좁혀지게 된다.

각 오브젝트를 관심사에 따라 적절하게 분리한 결과물이다. 정말 대단하다.

DAO를 사용하는 클래스들의 방식도 조금 변경되게 된다.

이때까지 CarDAO를 사용하는 클래스, 소위 말하는 클라이언트 들은 CarDAO를 직접 생성해서 사용했을 것이다.

하지만 위와같이 제 3자가 개입하므로써, CarDAO 클래스를 직접 생성할 필요가 없고, 제 3자를 생성해서 요청하는 방식으로 변경되게 된다.

# DI

그리고 방금, DaoFactory를 만드는 과정에서 DI를 발견할 수 있다.

먼저 DI를 알아보자

```
DI란 Dependency Injection의 약자로, 의존관계 주입 이라는 의미를 가지고 있다.
의존 관계란 별다른 뜻이 없다. 말 그대로 오브젝트가 서로 의존하고 있는 관계를 말한다.
객체 지향에서 의존하고 있다란 의미는, 하나의 오브젝트에서 다른 오브젝트를 사용할 때를 말한다.

A라는 클래스에서 B라는 클래스를 사용할 경우, A클래스는 B클래스에 의존하고 있다 라고 표현한다.(의존하고 있기 때문에 B클래스의 변경은 A클래스에 영향을 미친다.)


B클래스를 생성해서 A클래스에 넣어주는 과정, 이를 의존관계 주입이라고 보면 된다.
의존관계에 있는 오브젝트를 생성해서 주입해준다고 보면 되는것이다.

주입은 누가 해주느냐? 제 3자가 해준다. IOC의 개념이다.
결국 DI는 IOC의 세부적인 개념이다.

```

개념을 알고나니 DaoFactory에서 DI가 보이는가?

```
carDao.setCbConnection(dbConnection());
```

바로 이부분이다.
CarDAO는 DBConnection 인터페이스에 의존하고, getConnection 메서드는 의존체인 DBConnection을 생성해준다.
그리고 제 3자인 DaoFactory가 CarDAO가 의존하는 DBConnection을 생성하고, setter를 통해 넣어준다.
이것이 마치 제 3자가 메서드를 통해 주입해주는것과 같다고해서 이를 의존관계주입(DI)라고 부른다.

