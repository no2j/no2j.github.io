---
title : JAVA - ArrayList와 LinkedList
date : 2023-09-16 14:30:43 +09:00
categories : [Study,Java]
tags : [Java,자료구조]
---

## ArrayList와 LinkedList의 차이
<hr>

![차이](https://github.com/no2j/no2j.github.io/assets/106552182/be20fca5-99c3-4a3d-844c-237ad3e04419)

- ArrayList는 index가 있고, LinkedList는 각 원소마다 앞,뒤 원소의 위치값을 가지고 있다.
- 이러한 각각의 특징은 조회, 삽입, 삭제시에 성능의 차이를 발생시킨다.

<br>

`ArrayList와 LinkedList의 시간복잡도`
<br>

![성능차이](https://github.com/no2j/no2j.github.io/assets/106552182/9d384706-9bb4-45c1-87ee-e1a159e3cf6f)

### ArrayList
<hr>

![Array](https://github.com/no2j/no2j.github.io/assets/106552182/a159c821-257a-4c24-98f8-ef552548c2f7)

- ArrayList는 기본적으로 배열을 사용한다. 하지만 일반 배열과 차이점이 존재한다.
- 일반 배열은 처음에 메모리를 할당할 때 크기를 지정해주어야 하지만, ArrayList는 크기를 지정하지 않고 동적으로 값을 삽입하고 삭제할 수 있다.

<br>

**`조회`**
<hr>

- ArrayList는 각 데이터의 index를 가지고 있고 무작위 접근이 가능하기 때문에, 해당 index의 데이터를 한번에 가져올 수 있다.

<br>

**`데이터 삽입과 삭제`**
<hr>

- 데이터의 삽입과 삭제시 ArrayList는 그만큼 위치를 맞춰주어야 한다.
- 위 사진을 예로 들면 5개의 데이터가 있을 때 맨 앞의 0번째 인덱스를 삭제하였다면 나머지 뒤의 4개를 앞으로 한칸씩 이동해야 한다.
- 삽입과 삭제가 많다면 ArrayList는 비효율적이다.

<br><br>

### LinkedList
<hr>

![LinkedList](https://github.com/no2j/no2j.github.io/assets/106552182/bd589b4b-9a42-4722-af37-954672335079)

- LinkedList는 내부적으로 양방향의 연결 리스트로 구성되어 있어 참조하려는 원소에 따라 처음부터 정방향 또는 역순으로 순회 가능 (배열의 단점을 보완하기 위해 LinkedList가 고안되었다.)

<br>

**`조회`**
<hr>

- LinkedList는 순차적 접근이기 때문에 검색의 속도가 느리다.

<br>

**`데이터 삽입과 삭제`**
<hr>

- LinkedList는 데이터를 추가 및 삭제시 가리키고 있는 주소값만 변경해주면 되기 때문에 ArrayList에 비해 상당히 효율적이다.
- 위 사진을 예로 들면 2번째 값을 삭제하면 1번째 노드가 3번째 노드를 가리키게 하기만 하면 된다.

<br>

### 성능 비교
<hr>

![성능그래프](https://github.com/no2j/no2j.github.io/assets/106552182/e367bd3b-741f-4bdd-b533-fb5082016a1e)

- 조회(get)의 경우 ArrayList가 우위에 있지만, 삽입(add)/삭제(remove)의 경우 LinkedList가 뛰어난 성능을 보여준다.
- 소량의 데이터를 가지고 사용할 때는 큰 차이가 없지만 정적인 데이터를 활용하면서 `조회가 빈번`하다면 `ArrayList`를 사용하는 것이 좋고, 동적으로 `추가/삭제 요구사항이 빈번`하다면 `LinkedList`를 사용하는 것이 좋다.