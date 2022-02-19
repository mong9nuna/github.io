---
layout: post
title: '필수 자료구조'
author : 효준
date: 2019-04-19 10:00
tags: [basic]
image: /files/covers/blog.jpg
---

# Map vs HashMap

둘의 가장 큰 차이는 특정 키에 대한 값을 찾는 과정에서, HashMap은 이름 그대로 hash 테이블을 이용해서 키-값 관계를 유지하며, map은 red-black tree 알고리즘을 사용한다.

HashMap은 이상적으로 O(1)의 시간을 소요하지만, 실제로는 HashTable크기에 반비례하는 O(n) 의 시간을 소요한다. 큰 문제점이 없다면, HashTable의 크기는 저장되는 key-value pair의 20% 정도만 되어도 충분히 쓸만하다.
그리고 HashTable은 Key값을 Hashing해야 하기 떄문에, 특정한 객체를 key로 쓸 경우 별도의 적절한 Hash 생성함수를 만들어야 한다. 그리고 삽입,제거 과정이 가볍다.

Map의 경우 최악으로는O(n), 최상 O(logn)의 성능을 낸다. 그러나 red black tree 특성상 node rotation을 하는 과정에서 최악 log2n개의 노드의 값을 업데이트하는 부하가 있다. red black tree는 key ordered tree이기 때문에, key값이 특정한 구조체인 경우 key값의 비교함수나 비교연산자가 만들어져 있어야 한다.

# List vs Set

List와 Set의 차이는
List는 중복을 허용하고 순서가 유지되지만,
Set은 중복을 허용하지 않고 순서가 없다.

# Stack vs Queue

Stack의 경우 자료의 입출력을 한 곳으로 제한한 자료구조이다.

LIFO(LAST IN FIRST OUT)구조이고 push,pop을 이용하여 데이터를 넣고 뺀다.

Queue의 경우 자료의 입력과 출력을 양쪽으로 으로 제한한 자료구조

입력은 한곳에서만 들어오고 출력은 한곳에서만 나간다.

FIFO(FIRST IN FIRST OUT)구조이며 put,get을 이용하여 데이터를 넣고 뺀다.

컴퓨터 버퍼에서 주로 사용되며 마구 입력이 되었으나 처리를 하지 못할때, 버퍼(큐)를 만들어 대기시킨다.

일반적인 큐의 단점 : 큐에 빈 메모리가 남아있어도 꽉 차있는것으로 판단할 수 있음. rear가 배열의 끝에 도달했을 경우.

=> 개선된 원형 큐가 나옴.

원형 큐의 단점 : 메모리 공간은 잘 활용하나 배열로 구현되어 있기 때문에 큐의 크기가 제한되는 단점이 존재한다.

=> 링크드리스트 큐가 나옴.

링크드 리스트로 구현한 큐는 큐의 크기가 제한이 없고 삽입, 삭제가 편리하다.
