---
layout: post
title: '팩토리 메소드 패턴(Factory Method Pattern)'
author : 효준
date: 2019-04-29 13:00
tags: [basic]
image: /files/covers/blog.jpg
---

# 학습목표

+ 팩토리 메소드 패턴에서 템플릿 메소드 패턴의 사용됨을 안다.
+ 팩토리 메소드 패턴에서의 구조와 구현의 분리를 이해하고 구조와 구현의 분리의 장점을 안다.


# 요구사항

+ 게임 아이템과 아이템 생성을 구현해주세요.
    + 아이템을 생성하기 전에 데이터베이스에서 아이템 정보를 요청한다.
    + 아이템 생성 후 아이템 복제 등의 불법을 방지하기 위해 데이터 베이스에 아이템 생성 정보를 남긴다.
+ 아이템을 생성하는 주체를 ItemCreator로 짓는다.
+ 아이템은 item이란 인터페이스로 다룰 수 있도록 한다.
    + item은 use 함수를 기본 함수로 갖고 있다.
+ 현재 아이템의 종류는 체력회복 물약, 마력회복 물약이 있다.        


# 소스코드

```
package concrete;

import framewok.Item;
import framewok.ItemCreator;

public class Main {
    public static void main(String[] args) {
        ItemCreator itemCreator;
        Item item;
        itemCreator = new HpCreator();
        item = itemCreator.create();

        item.use();

        itemCreator = new MpCreator();

        item = itemCreator.create();

        item.use();

    }
}
-----------------
package framewok;

public interface Item {

    public void use();

}

-----------------
package framewok;

public abstract class ItemCreator {

    // 팩토리 메소드 -> 템플릿 메소드와 유사
    public Item create() {
        Item item ;

        //step 1
        requestItemsInfo();
        //step 2
        item = createItem();
        //step 3
        createItemLog();

        return item;
    }

    //아이템을 생성하기 전에 데이터베이스에서 아이템 정보를 요청한다.
    abstract protected void requestItemsInfo();

    //아이템 생성 후 아이템 복제 등의 불법을 방지하기 위해 데이터 베이스에 아이템 생성 정보를 남긴다.
    abstract protected void createItemLog();

    //아이템을 생성하는 알고리즘
    abstract protected Item createItem();

}

--------------

package concrete;

import framewok.Item;
import framewok.ItemCreator;

import java.util.Date;

public class HpCreator extends ItemCreator {
    @Override
    protected void requestItemsInfo() {
        System.out.println("데이터베이스에서 체력 회복 물약의 정보를 가져옵니다.");
    }

    @Override
    protected void createItemLog() {
        System.out.println("체력 회복 물약을 새로 생성 했습니다." + new Date());
    }

    @Override
    protected Item createItem() {
        //작업
        return new HpPotion();
    }
}

-----------------
package concrete;

import framewok.Item;
import framewok.ItemCreator;

import java.util.Date;

public class MpCreator extends ItemCreator {
    @Override
    protected void requestItemsInfo() {
        System.out.println("데이터베이스에서 마력 회복 물약의 정보를 가져옵니다.");
    }

    @Override
    protected void createItemLog() {
        System.out.println("마력 회복 물약을 새로 생성 했습니다." + new Date());
    }

    @Override
    protected Item createItem() {
        //작업
        return new MpPotion();
    }
}
---------------

package concrete;

import framewok.Item;

public class HpPotion implements Item {
    @Override
    public void use() {
        System.out.println("체력 회복");
    }
}
----------------

package concrete;

import framewok.Item;

public class MpPotion implements Item {
    @Override
    public void use() {
        System.out.println("마력 회복");
    }
}





```

# 결과

위와 같이 팩토리 메소드 패턴은 템플릿 메소드 패턴을 기본으로 
구현 시 소스를 크게 수정하지 않고 추가되는 기능이 있더라도 각각을 상속받아 사용하면 된다.