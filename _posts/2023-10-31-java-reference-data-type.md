---
title: Section 16 자바 프로그래밍의 참조 자료형
date: 2023-10-31
categories: [blog]
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



reference type,
primitive type

Planet jupiter = new Planet();
여기서 주피터는 참조변수(reference variable)가 됩니다.

int i = 5;
i 는 기본 변수(primitive variable) 입니다.

```java
class Animal {
    int id;
    Animal(int id){
        this.id = id;
    }
}


Animal dog = new Animal(12); 
Animal cat = new Animal(15); 
Animal nothing; // Stack 의 value 에는 null 이 나옵니다.

nothing = cat; // 이때 nothing 의 Stack-value 에는 1C 가 기록됩니다.

nothing.id = 10; // cat.id 도 10이 된다.

```


<div class="table-wrap">
<table class="center-table">
<tr><th colspan='3'  class="title-cell">Stack</th></tr>
  <tr>
    <th>Location</th>
    <th>Value</th>
    <th>variable-name</th>
  </tr>
  <tr>
    <td>A</td>
    <td>5</td>
    <td>i</td>
  </tr>
  <tr>
    <td>B</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>C</td>
    <td>1A</td>
    <td>dog</td>
  </tr>
  <tr>
    <td>D</td>
    <td>1C</td>
    <td>cat</td>
  </tr>
  <tr>
    <td>E</td>
    <td>1C</td>
    <td>nothing</td>
  </tr>
  <tr>
    <td>F</td>
    <td></td>
    <td></td>
  </tr>
</table>

Each Method has a seperate Stack		
		

<table class="center-table">
<tr><th colspan='2'  class="title-cell">Heap</th></tr>
  <tr>
    <th>Location</th>
    <th>Object</th>
  </tr>
  <tr>
    <td>1A</td>
    <td>Animal12</td>
  </tr>
  <tr>
    <td>1B</td>
    <td></td>
  </tr>
  <tr>
    <td>1C</td>
    <td>Animal15</td>
  </tr>
  <tr>
    <td>1D</td>
    <td></td>
  </tr>
  <tr>
    <td>1E</td>
    <td></td>
  </tr>
  <tr>
    <td>1F</td>
    <td></td>
  </tr>
</table>
</div>

Global Shared	
	

참조변수를 생성하면 메모리의 힙이라는 곳에 저장된다.
Animal dog = new Animal(12); 

dog 는 스택 어딘가에 변수이름으로 저장된다. 그리고 값에는 힙의 Location 이 주소가 저장된다. 그래서 참조변수라고 부르는 것이다.
dog 는 스택 어딘가에 변수이름으로 저장된다.

Stack 의 value 에는 실제 객체가 저장된 메모리의 경로(HEAP) 이 기록됩니다.