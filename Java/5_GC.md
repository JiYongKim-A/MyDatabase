# GC 

## 목차 
## 1. [들어가기전](#1-들어가기-전에)

## 2. [GC(Garbage Collection)](#2-gcgarbage-collection이란)

## 3. [GC의 메모리 해체 단계](#3-gc의-메모리-해체-단계-용어)

## 4. [GC관점 heap 메모리](#4-gc관점-heap-메모리-영역)

## 5. [GC 동작 과정](#5-gc의-동작-과정)

## 6. [전체 정리](#6-정리본)

<br>

---
## 1. 들어가기 전에

C/C++ 프로그래밍을 할 때 <U>**메모리 누수(Memory Leak)**</U>를 막기 위해 객체를 생성한 후 사용하지 않는 객체의 메모리를 <U>프로그래머가 직접 해제 해주어야 했다</U>.

>⇒ 하지만, JAVA에서는 JVM(Java Virtual Machine)이 구성된 JRE(Java Runtime Environment)가 제공되며, 그 구성 요소 중 하나인 <U>Garbage Collection이 자동으로 사용하지 않는 객체를 파괴한다</U>.

<br>

### GC를 해도 더이상 사용 가능한 메모리 영역이 없는데 계속 메모리를 할당하려고 한다면?

 OutOfMemoryError가 발생하여 WAS가 다운될 수도 있다.

> ⇒  행(Hang) 즉, 서버가 요청을 처리 못하고 있는 상태가 되는 것.

> ⇒ 따라서 규모 있는 JAVA 애플리케이션을 효율적으로 개발하기 위해서는 GC에 대해 잘 알아야한다.


<br>

### `stop-the-world`란?

stop-the-world란, GC를 실행하기 위해 JVM이 애플리케이션 실행을 멈추는 것.
    
>⇒ 어떤 GC 알고리즘을 사용하더라도 'stop-the-world'는 발생하게 되는데, 대개의 경우 GC 튜닝은 이 'stop-the-world' 시간을 줄이는 것이라고 한다.

<br>

### `Mark`란?

애플리케이션이 일시 중지되면 GC는 참조되고 있는 객체와 연결된 객체를 타고 이동하며 접근 가능한 객체를 식별하는 과정


<br>

### `Sweep`란? 

모든 객체 탐색이 끝나면 식별(Mark)되지 않은 객체들을 메모리에서 해제시키는 과정


<br>

---
## 2. GC(Garbage Collection)이란?

C/C++ 언어와 달리 <U>자바는 개발자가 명시적으로 객체를 해제할 필요가 없으며 이는 자바 언어의 큰 장점</U>이기도 하다.

<br>

GC의 정의 : 사용하지 않는 객체를 메모리에서 삭제하는 작업을 GC라고 한다.

>기본적으로 JVM의 메모리는 총 5가지 영역(class, stack, heap, native method, PC)으로 나뉘는데, GC는 힙 메모리만 다룬다.

<br>

### GC의 대상

1. 객체가 NULL인 경우 (ex. String str = null)
2. 블럭 실행 종료 후, 블럭 안에서 생성된 객체
3. 부모 객체가 NULL인 경우, 포함하는 자식 객체

GC는 Weak Generational Hypothesis 에 기반한다.

<br>

### Weak Generational Hypothesis

신규로 생성한 객체의 대부분은 금방 사용하지 않는 상태가 되고, 오래된 객체에서 신규 객체로의 참조는 매우 적게 존재한다는 가설 이다.

>이 가설에 기반하여 자바는 Young 영역과 Old 영역으로 메모리를 분할하고, 신규로 생성되는 객체는 Young 영역에 보관하고, 오랫동안 살아남은 객체는 Old 영역에 보관한다.

<br>

<br>

---
## 3. GC의 메모리 해체 단계 용어

1. Marking

    <img width="1095" alt="marking 이미지" src="">

    * 프로세스는 마킹을 호출하고 GC가 메모리가 사용되는지 아닌지를 찾아낸다.
    * 참조되는 객체는 파란색으로, 참조되지 않는 객체는 주황색으로 확인 가능
    * 모든 오브젝트는 마킹 단계에서 결정을 위해 스캔되어지게 되며 모든 오브젝트를 스캔하기 때문에 매우 많은 시간을 소모하게 된다.
        >* 보통 GC는 "루트(root)" 객체들을 시작으로 도달 가능한 객체를 추적하며 루트 객체는 전역 변수, 스택 프레임 등으로 정의될 수 있다.

<br>

2. Normal Delection (Sweep)

    <img width="1095" alt="Normal Delection 이미지" src="">

    * 참조되지 않는 객체를 제거하고, 메모리를 반환한다.
    * 메모리 Allocator는 반환되어 비어진 블럭의 참조 위치를 저장해 두고 새로운 오브젝트가 선언되면 할당되도록 한다.

<br>

3. Compacting (Defragmentation : 조각 모음)

    <img width="1095" alt="Compacting 이미지" src="">

    * 퍼포먼스를 향상시키기 위해, 참조되지 않는 객체를 제거하고 남은 참조되어지는 객체들을 묶는다.
    *  이들을 묶음으로서 공간이 생기므로 새로운 메모리 할당 시에 더 쉽고 빠르게 진행 할 수 있다.


---

## 4. GC관점 Heap 메모리 영역

<img width="1095" alt="GC관점 heap 메모리 영역 이미지" src="">

- Young 영역( Yong Generation 영역 )
    
    새롭게 생성한 객체의 대부분이 여기에 위치한다. 대부분의 객체가 금방 Unreachable(접근 불가능) 상태가 되기 때문에 매우 많은 객체가 Young 영역에 생성되었다가 사라진다. 
    
    이 영역에서 객체가 사라질때 **Minor GC** 가 발생한다고 말한다.
    
    <br>

- Eden 영역
   
    새로 생성된 객체가 할당되는 영역이다.
    Eden 영역이 꽉차면 GC가 발생하면서 Mark(참조 여부 식별), Sweep(메모리 해제) 과정이 일어난다.

    아직 사용중인 객체는 Survivor 영역으로 이동하며, Eden 영역은 비워진다.

    <br>

- Survivor 영역

    Eden 영역이 꽉 차게 되면, GC가 발생하면서 제거된 객체 외의 나머지 살아남은 객체는 다른 Survivor 영역으로 이동하게 된다.

    (한번의 Minor GC를 경험한 객체들이 저장되는 곳)

    <details>
    <summary>Survivor의 두 영역</summary>

    Survivor 두 영역중 하나는 반드시 비어있는 상태다. 만약 두 영역에 모두 데이터가 존재하거나, 사용량이 0이라면 정상적인 상황이 아니다.
    
    <br>

    Minor GC가 발생하면 Eden과 Survivor0에 살아있는 객체를 Survivor1로 복사한다.
    그리고 Survivor0과 Eden을 Clear한다.
    결과적으로 한번의 Minor GC에서 살아남은 객체만 Survivor1영역에 남는다.
    
    >  그리고 다음번 Minor GC가 발생하면 같은 방식으로 Eden과  Survivor1영역에서 살아있는 객체를 Survivor0로 복사하고 클리어한다. 결과적으로 Survivor0에만 살아있는 객체가 남게된다.
    
    <br>

    >  이렇게 반복적으로 Survivor0, Survivor1를 왔다갔다하다가, Survivor 영역에서 오래 살아남은 객체는 Old영역으로 옮겨진다. 

    <br>

    promotion 과정이 발생한다.

    > promotion 과정
    >계속해서 살아남은 객체들은 2개의 Survivor 영역을 이동하면서, 특정 age 값을 넘어가는 경우 Old Generation으로 이동하게 된다.

    <br>

    age를 이용하여 판별
    
    >객체들이 살아날때마다 age가 증가한다.

    <br>

    Survivor 영역은 왜 2개인 이유

    메모리의 외부 단편화 발생을 방지한다. 
    외부 단편화란, <U>메모리가 할당되고 해제되기를 반복하다보면 메모리 공간은 남지만 파편화되어있어 메모리를 할당할 수 없는 문제가 발생</U>한다. 그래서 두개의 Survivor 끼리 번갈아가며 메모리를 할당하며 이를 방지한다.

    </details>


<br>

- Old 영역( Old Generation 영역 )
    
    접근 불가능 상태로 되지 않아 Young 영역에서 살아남은 객체가 여기로 복사된다.
    
    대부분 Young 영역보다 크게 할당하며, 크기가 큰 만큼 Young 영역보다 GC는 적게 발생한다.
    
    이 영역에서 객체가 사라질 때 **Major GC(혹은 Full GC)** 가 발생한다고 말한다.
    
    <br>

- Permanet 영역
    
    Method Area라고도 한다.
    
    JVM이 클래스들과 메소드들을 설명하기 위해 필요한 메타데이터들을 포함하고 있으며 JDK8부터는 PermGen은 Metaspace로 교체된다.

<br>

---


## 5. GC의 동작 과정

[Minor GC의 동작 과정]

<img width="1095" alt="minor gc" src="">
- Young Generation 영역은 짧게 살아남는 메모리들이 존재하는 공간이다.

- 모든 객체는 처음에는 Young Generation에 생성되게 된다.

- Young Generation의 공간은 Old Generation에 비해 상대적으로 작기 때문에 메모리 상의 객체를 찾아 제거하는데 적은 시간이 걸린다. (작은 공간에서 데이터를 찾기 때문)

- 이 때문에 Young Generation 영역에서 발생되는 GC를 Minor GC라 불린다.

<br>
<br>

1. 어떠한 새로운 객체가 생성되면 Eden Space에 할당된다.
    <img width="1095" alt="GC의 동작 과정1" src="">

<br>

2. Eden Space가 가득차게 되면, minor garbage Collection이 시작된다.
    <img width="1095" alt="GC의 동작 과정2" src="">

<br>

3. 참조되는 객체들은 첫 번째 survivor(S0)로 이동되어지고, 비 참조 객체는 Eden space가 clear 될 때 반환된다.
    <img width="1095" alt="GC의 동작 과정3" src="">

<br>

4. 다음 minor GC 때, Eden space에서는 같은 일이 일어난다.

    비 참조 객체는 삭제되고 참조 객체는 survivor space로 이동하는 것
    
    >⇒ 하지만 이 케이스에서 참조 객체는 두 번째 survivor space로 이동하게 된다.
    
    >또한 최근 minor GC에서 첫 번째 survivor space로 이동된 객체들도 age가 증가하고 S1 공간으로 이동하게 된다.
    
    >한번 모든 surviving 객체들이 S1으로 이동하게 되면 S0와 Eden 공간은 Clear 된다. 
    
    <img width="1095" alt="GC의 동작 과정4" src="">

<br>

5. 다음 minor GC 때, 같은 과정이 반복 된다. 하지만 이번엔 survivor space들은 switch 된다. 참조되는 객체들은 S0로 이동하고 살아남은 객체들은 aged된다.

    그리고 Eden과 S1 공간은 Clear 된다.
    <img width="1095" alt="GC의 동작 과정5" src="">

<br>

6. 아래 그램은 promotion을 보여준다. minor GC 후 aged 오브젝트들이 일정한 age threshold(문지방)을 넘게 되면 그것들은 young generation에서 old generation으로 promotion 되어 진다. 
    <img width="1095" alt="GC의 동작 과정6" src="">

<br>

7. minor GC가 계속되고 계속해서 객체들이 Old Generation으로 이동된다.
    <img width="1095" alt="GC의 동작 과정7" src="">

<br>

<br>

[Major GC]

<img width="1095" alt="major gc" src="">

- Old Generation은 길게 살아남는 메모리들이 존재하는 공간이다.

- Old Generation의 객체들은 Young Generation에 의해 시작되었으나, GC 과정 중에 제거되지 않은 경우 age 임계값이 차게되어 Old Generation으로 이동된 객체들이다.

- 그리고 Major GC는 객체들이 계속 Promotion되어 Old 영역의 메모리가 부족해지면 발생하게 된다.

- Major GC는 Full GC라고도 불린다.

<br>


1. 객체의 age가 임계값(여기선 8로 설정)에 도달할 시

    <img width="1095" alt="major gc1" src="">

<br>

2.  이 객체들은 Old Generation 으로 이동되며 이를 promotion 이라 부른다.

    <img width="1095" alt="major gc2" src="">

<br>

3. 위의 과정이 반복되어 Old Generation 영역의 공간(메모리)가 부족하게 되면 Major GC가 발생되게 된다.

    <img width="1095" alt="major gc3" src="">

<br>

Major GC는 Old 영역은 데이터가 가득 차면 GC를 실행하는 단순한 방식이다. 

> Old 영역에 할당된 메모리가 허용치를 넘게 되면, Old 영역에 있는 모든 객체들을 검사하여 참조되지 않는 객체들을 한꺼번에 삭제하는 Major GC가 실행되게 된다.

> 하지만 Old Generation은 Young Generation에 비해 상대적으로 큰 공간을 가지고 있어, 이 공간에서 메모리 상의 객체 제거에 많은 시간이 걸리게 된다.


>⇒ Young 영역은 일반적으로 Old 영역보다 크키가 작기 때문에 GC가 보통 0.5초에서 1초 사이에 끝난다.

>그렇기 때문에 Minor GC는 애플리케이션에 크게 영향을 주지 않는다.

>⇒ 하지만 Old 영역의 Major GC는 일반적으로 Minor GC보다 시간이 오래걸리며, Young 영역을 참조할 수도 있기 때문에 10배 이상의 시간을 사용한다.

<br> 

[Stop-The-World 문제가 발생]

Major GC가 일어나면 Thread가 멈추고 Mark and Sweep 작업을 해야 해서 CPU에 부하를 주기 때문에 멈추거나 버벅이는 현상이 일어나기 때문이다.

따라서 자바 개발진들은 끊임 없이 가비지 컬렉션 알고리즘을 발전해 왔다.

<br>

[Minor GC와 Major GC의 차이점]

|  | minor GC | major GC |
| --- | --- | --- |
| 대상 | Yong generation | Old Generation |
| 실행 시점 | Eden 영역이 꽉 찬경우 | Old 영역이 꽉 찬 경우 |
| 실행 속도 | 빠르다 | 느리다 |


<br>
---



## 6. 정리본

* GC가 무엇인지 설명해 주세요
    * 답변 : Garbage Collection이란 JVM에서 메모리를 메모리 누수 방지 목적으로 사용자 동적 메모리인 Heap 영역에서 참조 되어지지 않는 메모리 영역을 반환하는 것을 의미합니다. 

        이는 프로그래머가 명시적으로 메모리를 해제하지 않아도 되도록 도와주는 중요한 기능으로 이를 통해 프로그래머는 메모리 관리에 대한 부담을 덜 수 있고, 메모리 누수로 인한 성능 저하나 예기치 못한 동작을 방지할 수 있습니다.

<br>

* GC의 대상은 어떤것이 있는지 설명해 주세요
    * 답변 : GC의 대상으로는 대표적으로 
      1. 객체가 NULL인 경우 (ex. String str = null)
      2. 블럭 실행 종료 후, 블럭 안에서 생성된 객체
      3. 부모 객체가 NULL인 경우, 포함하는 자식 객체
      가 있습니다.

<br>

* Yong 영역의 eden 영역과 survivor영역에 대해 설명해 주세요
    * 답변 : eden영역은 새로 생성된 객체가 할당되는 영역이며 이곳에서 minor GC가 일어나면 아직 사용중인 객체는 Survivor 영역으로 이동하고 Eden 영역은 비워지게 됩니다.

<br>

* Old 영역에 대해 설명해 주세요
    * 답변 : Old 영역은 Yong 영역에서 Minor GC가 이루어진 후에 아직 사용중인 객체가 옮겨지는 곳으로 대부분 Young 영역보다 크게 할당하며, 크기가 큰 만큼 Young 영역보다 GC는 적게 발생하는 영역 입니다.

<br>

* Minor GC가 무엇인지 설명해 주세요
    * 답변 : Minor GC란 Young Generation 영역은 짧게 살아남는 객체들이 존재하는 공간으로 모든 객체는 처음에는 Young Generation에 생성되게 됩니다 이때 Young Generation 영역의 Eden 영역이 꽉 찬경우 발생되는 GC를 Minor GC라 부릅니다.


<br>

* Major GC가 무엇인지 설명해 주세요
    * 답변 : Major GC란 Old Generation영역은 상대적으로 Yong 영역보다 오래 살아남은 객체들이 존재하는 곳으로 Old영역의 데이터가 가득 찬 경우 발생되는 GC를 Major GC라 부릅니다.

<br>