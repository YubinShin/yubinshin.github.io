---
title: Section 18 자바 Array, ArrayList
date: 2023-11-02
categories: [blog, java]
tags: [java]
---

<style>
  /* 기본 스타일 */
  .center-table {
    margin: 0 auto; /* 가운데 정렬을 위한 스타일 */
    text-align: center; 
    width : 50%;
    
  }
  .center-table td {
    width : 25%;
  }
    .center-table td:last-child {
    width : 30%;
  }
  div.content .table-wrapper>table {
    min-width: 90%;
    }
  .table-wrapper{
            width:100%;
            margin: 0;
            padding:0;
    }
  .table-wrap {
    display: flex;
    width: 100%;
    justify-content: space-around;
    flex-wrap: nowrap;
    flex-direction: row;           
  }

    .title-cell{
        background-color : orange;
    }

  @media (max-width: 768px) {
    .center-table {
      width: 100%; 
    }
    .table-wrap {
      flex-direction : column;
      justify-content: center; 
    }
  }
</style>


Collection 안에 List, Queue, Set, Map 가 있다.

## ArrayList vs LinkedList

- ArrayList , LinkedList, Vector 가 있다.
- 순서가 있는 데이터의 집합으로 데이터의 중복을 허용한다.

ArrayList, Vector 는 배열이 기본이다.(Read 가 빠르고, Write 가 느리다)

LinkedList 는 링크드리스트가 기본이다. (Read 가 느리고, Write 가 빠르다)



## Syncronized Collections VS Concurrent Collections 

Vector 가 별로라서 ArrayList가 생겼다.
멀티스레드 환경에선 syncronized method 를 쓰면 15개의 스레드중 1개만 메서드를 실행 할 수 있다.

Vector 의 메소드는 모두 Syncronized 이다. 그래서 안전하지만 느리다.

ArrayList의 메소드들은 기다리지 않는다. 그래서 안전이 필요하지 않은 상황이면 ArrayList 를 쓰는게 낫다.

## List

### CRUD
#### 리스트 생성하기

`List<String> words = List.of("Apple", "Bat", "Cat"); `

List.of()로 만든 리스트는 Immutable 이다.

ArrayList, LinkedList, Vector 생성자로 만들면 mutable 로 생성된다.

```java
List<String> words = List.of("Apple", "Bat", "Cat"); 
// 안됨 words.add("Dog") 
List<String> wordsArrayList = new ArrayList<String>(words);
List<String> wordsLinkedList = new LinkedList<String>(words);
List<String> wordsVector = new Vector<String>(words);
```
#### Read

get(), indexOf() 등

#### add, addAll (요소 추가하기)

```java
wordsArrayList.add("Elephant"); // 인덱스를 명시 하지 않으면 맨 뒤에 추가됨
wordsArrayList.add(2, "Ball"); 

List<String> newList = List.of("Yak", "Zebra"); 

wordsArrayList.addAll(newList); // 인덱스를 명시 하지 않으면 맨 뒤에 추가됨
wordsArrayList.addAll(2, newList);
```

#### set (요소 교체하기)

```java
wordsArrayList.set(6, "Fish"); // 특정 위치에 있는 요소 교체하기
```

#### remove (요소 제거하기)

```java
wordsArrayList.remove(2); // 특정 위치에 있는 요소 제거하기
wordsArrayList.remove("Dog"); // 특정 내용의 요소 제거하기
```

### Loop

1. for 반복문
   
  ```java
  List<String> words = List.of("Apple", "Bat", "Cat");

  // for loop
  for(int i = 0; i < words.size; i++){
    System.out.println(words.get(i));
  }

  // Enhanced for loop
  for(String word : words){
    System.out.println(word);
  }
  ```

2. iterator (반복자)

  ```java
  Iterator wordIterator = words.iterator();
  while( wordsIterator.hasNext()){
      System.out.println(wordsIterator.next());
  }
  ```

<details markdown="block"><summary> 숙제</summary>

```java
List<Integer> integers = List.of(1,2,3,4,5);

for(int i = 0; i < integers.size(); i++){
  System.out.println(integers.get(i));
}

// Enhanced for loop
for(Integer integer : integers){
  System.out.println(integer);
}

Iterator iteratedIntgers = integers.iterator();
while(iteratedIntgers.hasNext()){
  System.out.println(iteratedIntgers.next());
}
```

</details>

왜 리스트를 반복하는 방법이 세가지나 될까요?
왜냐하면, 특정 작업동안에는 특정 루프를 사용해야하기 때문이죠

"at" 로 끝나는 단어를 print 해보기
우리가 이런 상황에서 Iterator 를 쓰려고 했으면 코드가 엄청 길었을겁니다. (이것보다 더 좋은 방법인 Stream 은 나중에 배울게여)

  ```java
    List<String> words = List.of("Apple", "Bat", "Cat");
    List<String> wordsAl = new ArrayList<String>(words);
    //  List<String> wordsAl = new ArrayList<>(words); 이렇게 해도 됩니다

    for (int i = 0; i < wordsAl.size(); i++) {
      String item = wordsAl.get(i);
      if (item.endsWith("at")) System.out.println(wordsAl.get(i));
    }

    for(String word:words){
      if (word.endsWith("at")) System.out.println(word);
    }
 
  ```

그런데 만약 우리가 "at" 로 끝나는 단어를 delete 하고 싶다면, enhanced for loop 를 찾을 수 없습니다.
