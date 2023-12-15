---
title: 혼자 공부하는 컴퓨터구조 & 운영체제 - 교착 상태
date: 2023-12-15
categories: [blog, etc]
tags: [os, deadlock]
---

![https://yubinshin.s3.ap-northeast-2.amazonaws.com/2023-12-15-operating-system/deadlock.gif](https://yubinshin.s3.ap-northeast-2.amazonaws.com/2023-12-15-operating-system/deadlock.gif)

## 식사하는 철학자 문제

dining philosophers problem
- 철학자 : 프로세스
- 포크 : 자원
- 생각 : 자원을 기다리며 포크를 든 채 대기

0. 포크를 양손에 들고 있어야만 식사가 가능하다.
1. 일정 시간 생각을 한다.
2. 왼쪽 포크가 사용 가능해질 때까지 대기한다. 만약 사용 가능하다면 집어든다.
3. 오른쪽 포크가 사용 가능해질 때까지 대기한다. 만약 사용 가능하다면 집어든다.
4. 양쪽의 포크를 잡으면 일정 시간만큼 식사를 한다.
5. 오른쪽 포크를 내려놓는다.
6. 왼쪽 포크를 내려놓는다.
7. 다시 1번으로 돌아간다.

모든 철학자가 동시에 식사를 하려고 하면 아무도 식사를 할 수 없습니다. (기아 상태)

다들 왼쪽에 포크를 쥐고 있기 때문에 오른쪽 포크를 쥘 수 있는 사람이 없거든요.

<!-- 이렇게 일어나지 않을 사건을 기다리며 진행이 멈춰버리는 상황을 **교착 상태** 라고 합니다. -->


## 교착 상태란?

2개 이상의 프로세스가 각자 가지고 있는 자원을 들고 무작정 기다리느라  진행이 멈춰버리는 상황을 **교착상태**(deadlock)라고 합니다. 

교착 상태가 발생하면 어떤 프로세스도 더 이상 실행할 수 없게 됩니다.

![https://yubinshin.s3.ap-northeast-2.amazonaws.com/2023-12-15-operating-system/deadlock-figma.png](https://yubinshin.s3.ap-northeast-2.amazonaws.com/2023-12-15-operating-system/deadlock-figma.png)

## 교착상태를 표현하는 법

**자원 할당 그래프**로 교착 상태를 표현합니다.

![https://yubinshin.s3.ap-northeast-2.amazonaws.com/2023-12-15-operating-system/Deadlock-avoidance.png](https://yubinshin.s3.ap-northeast-2.amazonaws.com/2023-12-15-operating-system/Deadlock-avoidance.png)

자원 할당 그래프가 **원 모양**을 띄면 교착 상태가 발생할 수 있습니다.

## 교착 상태의 발생 조건

4가지 조건을 모두 만족 하면 교착 상태가 발생할 수 있습니다.

#### 상호 배제(Mutual Exclusion)

한 번에 하나의 프로세스만 자원을 사용할 수 있다면

![https://yubinshin.s3.ap-northeast-2.amazonaws.com/2023-12-15-operating-system/mutual_exclusion.png](https://yubinshin.s3.ap-northeast-2.amazonaws.com/2023-12-15-operating-system/mutual_exclusion.png)

#### 점유와 대기(hold and wait)

어떠한 자원을 할당 받았을 때 돌려놓지 않고 계속 차지하고 있다면 

![https://yubinshin.s3.ap-northeast-2.amazonaws.com/2023-12-15-operating-system/hold.png](https://yubinshin.s3.ap-northeast-2.amazonaws.com/2023-12-15-operating-system/hold.png)

#### 비선점(non preemptive)

강제로 다른 프로세스의 자원을 빼앗을 수 없다면

![https://yubinshin.s3.ap-northeast-2.amazonaws.com/2023-12-15-operating-system/nonpreem.jpg](https://yubinshin.s3.ap-northeast-2.amazonaws.com/2023-12-15-operating-system/nonpreem.jpg)

#### 원형 대기(circular wait)

자원 대기 그래프가 원형을 그린다면

![https://yubinshin.s3.ap-northeast-2.amazonaws.com/2023-12-15-operating-system/circular.png](https://yubinshin.s3.ap-northeast-2.amazonaws.com/2023-12-15-operating-system/circular.png)

## 교착상태를 해결하는 법

예방, 회피, 검출 & 회복, 무시하기 4가지 방법이 있습니다.

### **예방**

교착 상태의 발생 조건 4가지 중 하나라도 충족하지 못하게 하는 겁니다. 

1. **상호 배제를 없애기**
   
    현실적으로 불가능합니다.<br/>
    강아지 사진을 프린트하면서 동시에 나무 사진을 프린트 할 수 없잖아요?

2. **점유와 대기를 없애기**
   
   가능하지만 비효율 적입니다.<br/>
   철학자들에게 필요한 포크를 한번에 다 들수 있는게 아니면 아무 일도 하지 말라고 하는겁니다.<br/>하지만 자원의 활용률이 낮아집니다.
    ![https://yubinshin.s3.ap-northeast-2.amazonaws.com/2023-12-15-operating-system/deadlock-solution.png](https://yubinshin.s3.ap-northeast-2.amazonaws.com/2023-12-15-operating-system/deadlock-solution.png)

3. **비선점 조건 없애기** 
   
   가능은 하지만 어렵습니다.<br/>
   강아지 사진을 프린트하고 있는데 갑자기 워드 프로세서를 켜야한다고 프린터가 멈추면 당황스럽겠죠?

4. **원형 대기 조건 없애기**
   
   원형 대기 조건을 없애는 건 가능 하지만 최선은 아닙니다.<br/>
   철학자들을 원탁에 앉히는게 아니라 일렬로 앉힙니다. <br/>그리고 백 만개의 포크에 하나하나 번호를 붙인 뒤 오름 차순으로 포크를 드는 규칙을 줍니다. <br/>교착 상태가 발생하진 않겠지만 자원에 하나하나 번호를 붙이는 건 컴퓨터에 굉장히 부담이 큽니다. <br/>또한 번호가 뒤 쪽에 있는 자원은 활용률이 떨어지겠죠.
   

### **회피**

교착 상태 회피는 자원을 교착 상태가 발생하지 않을 정도로만 조심 조심 할당하는 겁니다.

이 관점에선 교착 상태가 한정된 자원의 무분별한 분배 때문에 생긴다고 보거든요.

교착 상태가 발생할지 않고 모든 프로세스가 정상적으로 자원을 할당받고 종료할 수 있는 상태를 **안전 상태** 라고 부릅니다. 반대로, 교착상태가 발생할 수도 있는 상태는 **불안전 상태**라고 부릅니다.

**안전 순서열**은 교착 상태 없이 안전하게 프로세스들에 자원을 할당할 수 있는 순서를 의미 합니다. 

운영체제가 **안전 순서열**에 따라 교착 상태가 발생하지 않게끔 자원을 할당하는 걸 **교착 상태 회피**라고 할 수 있습니다.

### **검출 & 회복**

교착 상태 발생을 인정하고 사후에 조치하는 방식입니다.

프로세스들이 자원을 요구할 때마다 그때그때 모두 할당하다가 교착상태가 발생하면 조치를 취합니다.

1. **선점을 통한 회복**

    강제로 빼앗아서 한 프로세스 씩 자원을 몰아줍니다.

    ![https://yubinshin.s3.ap-northeast-2.amazonaws.com/2023-12-15-operating-system/deadlock_solution_preem.jpg](https://yubinshin.s3.ap-northeast-2.amazonaws.com/2023-12-15-operating-system/deadlock_solution_preem.jpg)

2. **강제 종료를 통한 회복**
   
   가장 확실한 방법입니다. 
   
   운영체제가 교착 상태에 놓인 프로세스를 모두 강제 종료하는 방법도 있고, 하나 씩 종료하는 방법도 있습니다. 

   **첫 번째**는 가장 확실한 방법이지만 그만큼 많은 프로세스의 작업 내역을 잃게 될 가능성이 있습니다.

    ![https://yubinshin.s3.ap-northeast-2.amazonaws.com/2023-12-15-operating-system/adobe.jpg](https://yubinshin.s3.ap-northeast-2.amazonaws.com/2023-12-15-operating-system/adobe.jpg)

   **두 번째**는 작업 내역을 잃는 프로세스는 최대한 줄일 수 있지만 교착 상태가 사라졌는지 계속 확인해보는 과정에서 오버헤드를 야기합니다.
    <iframe src="https://giphy.com/embed/2imYjutn9uvAGR2ILU" width="480" height="270" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/natgeowild-nat-geo-wild-meerkat-lookout-2imYjutn9uvAGR2ILU"></a></p>
    
### 무시하기

교착 상태가 발생했다는 것 자체를 무시하는 타조 알고리즘을 사용합니다. ^^

의외로 이 방식이 적합할 때도 있다고 하네요.

![https://yubinshin.s3.ap-northeast-2.amazonaws.com/2023-12-15-operating-system/ostrich.gif](https://yubinshin.s3.ap-northeast-2.amazonaws.com/2023-12-15-operating-system/ostrich.gif)