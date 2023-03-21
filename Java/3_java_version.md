# Java 버전별 특징

## 목차 
## 1. [JRE?](#1-jre-java-runtime-environment)

## 2. [JDK?](#2-jdk-java-development-kit)

## 3. [자바의 하위호환성](#3-자바의-하위-호환성)

## 4. [자바 7, 8, 11 특징](#4-자바의-버전별-특징-7-8-11)

## 5. [전체 정리](#5-정리본)

<br>

---
## 1. JRE (Java Runtime Environment)
JRE(Java Runtime Environment)란?
* 정의 : JRE란 자바를 실행시키기 위해 필요한 라이브러리 및 기타 필수 파일을 가지고 있는 실행환경 입니다.
  
    * JVM이 자바프로그램을 실행시킬 때 반드시 필요한 라이브러리 및 기타 필수 파일을 가지고 있습니다.
    *  만약 자바 프로그램 개발이 아닌 단순히 자바 프로그램을 실행만 하려면 JRE만 다운받아 설치하면 됩니다.

<br>

 * 구성요소
    
    <img width="843" alt="스크린샷 2023-03-20 오전 10 13 51" src="https://user-images.githubusercontent.com/81874493/226295523-bd5353d2-4d7e-4eea-9083-fd05223158e3.png">


      * Class Loader
        * Java 클래스로더는 Java 프로그램의 실행에 필요한 모든 클래스를 동적으로 로드 합니다. Java 클래스는 필요 시에만 메모리에 로드 되므로, JRE는 클래스로더를 사용하여 요청 시에 이 프로세스를 자동화 합니다.
        
        <br>

      * Bytecode Verification
        * 바이트코드 검증기는 인터프리터에 전달되기 전에 Java 코드의 형식과 정확성을 보장 합니다. 코드가 시스템 무결성 또는 액세스 권한을 위반하는 경우, 클래스는 손상된 것으로 간주되어 로드되지 않습니다.
        
        <br>

      * Interpreter
        * 바이트코드의 로드에 성공한 후, Java 인터프리터는 Java 프로그램이 기본 시스템에서 기본적으로 실행될 수 있도록 해주는 JVM 인스턴스를 작성합니다.
        
        <br>

      * JCL(Java Class Libraries)
        * JCL (Java Class Libraries)은 JVM(Java Virtual Machine) 언어가 런타임 에 호출할 수 있으며 동적로드가 가능한 라이브러리 세트입니다 . 

        <br>

      * JVM(Java Virtual Machine)
        * OS에 종속받지 않고 CPU 가 Java를 인식,바이트 코드를 실행하는 실행할 수 있게 하는 가상 컴퓨터이다.
        
        <br>

* 동작 과정
  * JRE는 JDK로 작성된 Java 코드를 JVM에서 이의 실행에 필요한 필수  라이브러리와 연결한 후 결과 프로그램을 실행하는 JVM의 인스턴스를 작성 합니다.
  
    JVM은 다수의 운영체제에서 사용 가능하기 때문에 JDK를 사용하여 작성된 프로그램이 운영체제에 맞는 수정 없이 어떠한 운영체제에서도 실행이 가능하게 됩니다. 

<br>

---
## 2. JDK (Java Development Kit)
JDK (Java Development Kit)란?
* 정의 : JDK (Java Development Kit)란 Java로 소프트웨어를 개발할 수 있도록 여러 기능들을 제공하는 패키지(키트)입니다.

<br>

* 구성요소 
  
  <img width="1095" alt="스크린샷 2023-03-20 오전 11 04 31" src="https://user-images.githubusercontent.com/81874493/226295561-605f636a-f63a-4112-9ea5-d667d861db81.png">

  * 구성요소
    * apt :어노테이션 툴
    * appletviewer : 웹브라우저 없이 자바 애플릿을 실행하고 디버깅하기 위한 툴
    * javac : 자바 컴파일러. 자바 소스파일을 바이트코드로 변환
    * java : javac가 만든 클래스 파일을 해석 및 실행
    * jar : 서로 관련있는 클래스 라이브러리들과 리소스를 하나의 파일로 묶어주는 툴
    * jdb : 자바 디버깅 툴
    * JRE(Java Runtime Enviroment) : Java가 동작하는데 필요한 JVM, 라이브러리 등 다양한 파일들을 포함 합니다.

    <br>

* JDK 종류
  * Java SE(Java Standard Edition) : 가장 기본이 되는 표준 에디션의 자바 플랫폼으로 자바 언어의 핵심 기능을 제공
   

    <br>

  * Java EE(Java Enterprise Edition) : 대규모 기업용 에디션. SE확장판(대형 네트워크환경 프로그램 개발시)

  
    <br>
    
  * Java ME(Java Micro Edition) : 피쳐폰, PDA폰, 셉톱박스, 프린터와 같은 작은 임베디드 기기들 같은 작은 기기를 다루는데 이용하는 에디션


    <br>

  * JavaFX : 가볍고 예쁜 그래픽 사용자 인터페이스(GUI)를 제공하는 에디션
 

<br>

* JDK 버전 표기
    
    <img width="705" alt="스크린샷 2023-03-20 오전 11 35 15" src="https://user-images.githubusercontent.com/81874493/226295637-1881070b-a900-4b20-a4d8-95fce3d151ab.png">



<br>

---
## 3. 자바의 하위 호환성
Java는 하위 호환성이 매우 높아 Java 5 또는 8 프로그램이 Java 8-17 가상 머신에서 실행되도록 보장된다.

이 때문에 특정 버전의 Java만 공부할 필요는 없다

하지만, 반대로 java 17 기능에 의존하는 코드가 java 8 JVM에서는 컴파일 되지 않는다. (java.lang.UnsupportedClassVersionError)



<br>

---

## 4. 자바의 버전별 특징 (7, 8, 11) 

### Java SE 7
[대표적 특징]
*  Diamond Operator <>
    ```java
    //Java SE 7 버전 이전
    List<Integer> list = new ArrayList<Integer>();


    //Java SE 7 버전 이후
    List<Integer> list = new ArrayList<>();
    ```
    General Class 초기화 시 다이아몬드 연산자를 활용한 Type Reference(타입 추론)지원 하게
    되었습니다.

    <br>
* 다중 Exception Catching
    ```java
    //Java SE 7 버전 이전
        try{
            // code
        } catch (FirstException ex) {
            log.info(ex);
            throw ex;
        } catch (SecondException ex) {
            log.info(ex);
            throw ex;
        }

    //Java SE 7 버전 이후
        try{
        //code
        } catch (FirstException | SecondException ex) {
            log.info(ex);
            throw ex;
        }

    ```
    하나의 catche 블록에서 여러개의 예외처리가 가능하게 되었습니다.

<br>

### Java SE 8

[대표적 특징]

* Lambda Expression
    ```java
    //Java SE 8 버전 이전
    Runnable runnable = new Runnable(){
    
    @Override
    public void run(){
        System.out.println("Hello world !");
        }
    };

    //Java SE 8 버전 이후
    Runnable runnable = () -> System.out.println("Hello world!");
    ```
    메소드를 지칭하는 명칭 없이 구현부를 선언하는 익명 메소드 생성 문법입니다.
    
    별도의 익명 클래스를 만들어서 선언하던 방식을 람다를 통해 대폭 간소화할 수 있으며, 함수형 프로그래밍, 스트림 API 그리고 컬럭션 프레임워크의 개선 등에 영향을 주었습니다.
    
<br>

* Method Reference
    |lambda|Method Reference|
    |---|---|
    |(Soccer s) -> s.getGoal()|Soccer::getGoal|
    |() -> Thread.currentThread().dumpStack()|Thread.currentThread()::dumpStack|
    |(str, i) -> str.substring(i)|String::substring|
    |(String s) -> System.out.println(s)|System.out::println|

    메소드 레퍼런스는 특정 메서드만을 호출하는 람다의 축약형입니다.
    
    메소드 레퍼런스는 하나의 메서드를 참조하는 람다를 편리하게 표현할 수 있는 문법으로 간주할 수 있습니다.

    <br>

* Interface Default Methods
  ```java
  public interface Repository{
    // 일반적 인터페이스 메소드 정의
    public void save(int a);

    // default 메소드를 통해 인터페이스에서 메소드 구현 가능
    public default void print(int a){
      System.out.println(a);
    }
  }

  //default 메소드가 구현된 인터페이스를 상속 받을 수 있다.
  public interface MemberRepository extends Repository{ 
  }
  ```
  
  Java SE 8에서는 인터페이스에 디폴트 메소드(Default Methods)라는 것이 추가되어 구현 내용도 인터페이스에 포함시킬 수 있습니다.

  또한, 디폴트 메소드를 구현한 인터페이스를 상속 받을 수 있습니다.

  <br>

* Null 처리 Optional 추가
  
  ```java
  String a = null;
  Optional<String> nullable = Optional.ofNullable(a);
  Boolean result = nullable.orElseThrow(Exception::new).equals("A");
  ```
  Java SE 8부터는 Optional이라는 구조체를 제공해서 이전보다 간편하게 NPE(Null Pointer Exception) 이슈에 대응할 수 있습니다.

  <br>

* 날짜와 시간 API
  * `java.time.Clock`
  * `java.time.LocalDate`
  * `java.time.ZoneId`
   
    등과 같은 새로운 날짜 API가 추가 되었습니다.
  
  <br>

*  Stream API
    ```java
    List<String> list = Arrays.asList("java7","java8");
    list.stream()
    .filter(s -> s.startWith("j"))
    .map(String::toUpperCase)
    .sorted()
    .forEach(System.out::println);
    ```
    Java SE 8부터는 Stream API를 통해 기존 컬렉션 프레임워크를 이용할 때보다 간결하게 코드 작성이 가능합니다.

    또한 병렬처리, 스트림 파이프라인을 통해 하나의 문장으로 다양한 처리 기능을 구현할 수 있습니다.
  
  <br>

* Heap permanent Generation 제거
  
  ![캡처1](https://user-images.githubusercontent.com/81874493/226532640-5267524c-3ae1-4440-b68b-23cb1dfa8f79.PNG)
  (Java SE 8 이전)
  
  <br>
  
  ![캡처2](https://user-images.githubusercontent.com/81874493/226532675-ddd41b67-8e11-45f1-8a59-98627161f710.PNG)
  (Java SE 8 이후)
  
  Java 8가 나오면서 JVM 영역에서 변화가 있었다.
  JVM의 여러 메모리 영역 중에
  Permanent Generation 메모리 영역이 없어지고 Metaspace 영역이 생겼다

  * Java Heap
    * Pergen에 있는 클래스의 인스턴스 저장.
    * -Xms (min), -Xmx(manx)로 사이즈 조정
  
  * PermGen
    * 클래스와 메소드의 메타데이터 저장
    * 상수 풀 정보
    * JVM, JIT 관련 데이터 저장
    * -XX : Permsize(min), XX:MaxPermSize(max)로 사이즈 조정.

  * Metaspace
    * Metaspace는 Java의 Classloader가 현재까지 로드한 Class들의 Metadata가 저장되는 공간이다.

    * 중요한 건 Heap 영역이 아니라 Native 메모리 영역에 위치한다.

    * Default로 제한된 크기를 가지고 있지 않다.
    그래서 필요한 만큼 계속 늘어난다.

    <br>
    
    ![캡처3](https://user-images.githubusercontent.com/81874493/226532935-a3e76430-61b9-49a2-864a-dae0336f9706.PNG)


    Perm 영역은 보통 Class의 Meta 정보나 Method의 Meta 정보, Static 변수와 상수 정보들이 저장되는 공간으로 흔히 메타데이터 저장 영역이라고도 한다. 
    
    이 영역은 Java 8 부터는 Native 영역으로 이동하여 Metaspace 영역으로 변경되었다. 
    (다만, 기존 Perm 영역에 존재하던 Static Object는 Heap 영역으로 옮겨져서 GC의 대상이 최대한 될 수 있도록 하였다)

  <br>

### Java SE 11
[대표적 특징]
Oracle JDK와 OpenJDK가 통합되었으며 Oracle JDK 가 유료 모델로 전환되었습니다.

* 가장 커다란 변화는 바로 라이선스 부분이다. Java SE 11 부터 Oracle JDK 의 독점 기능이 오픈 소스 버전인 OpenJDK 에 이식된다. 이는 다시 말해 Oracle JDK 와 OpenJDK 가 완전히 동일해진다는 뜻이다.

<br>

---

## 5. 정리본

* JRE, JDK 자바의 버전별 특징

<br>

* JRE의 정의와 구조에 대하여 설명해주세요.
    * 답변 :  JRE란 자바를 실행시키기 위해 필요한 라이브러리 및 기타 필수 파일을 가지고 있는 실행환경 이며 구조로는

      >* Class Loader        

      >* Bytecode Verification
   
      >* Interpreter
 
      >* JCL(Java Class Libraries)
     
      >* JVM(Java Virtual Machine) 
        
      JRE에 포함됩니다.

<br>


* JDK의 정의와 구조에 대하여 설명해주세요.
    * 답변 :  JDK (Java Development Kit)란 Java로 소프트웨어를 개발할 수 있도록 여러 기능들을 제공하는 패키지(키트)이며 구조로는
    
      >* java : javac가 만든 클래스 파일을 해석 및 실행
      >* jar : 서로 관련있는 클래스 라이브러리들과 리소스를 하나의 파일로 묶어주는 툴
      >* jdb : 자바 디버깅 툴

      와 같은  Dev Tool 과

      >* JRE(Java Runtime Environment)

      가 포함됩니다.


<br>


* Java SE 7, 8, 11 버전의 특징에 대하여 설명해주세요.
    * 답변 : 

      * JAVA SE 7
        * 다이아몬드 연산자를 활용한 __<U>타입 추론 지원</U>__
        * __<U>다중 예외 처리</U>__ 가 가능
        
  
      * Java SE 8
        * __<U>Lambda Expression</U>__
        * 람다의 표현 문법인 __<U>Method Reference</U>__
        * 인터페이스의 __<U>Default method</U>__
        * Null 처리에 사용되는 __<U>Optional</U>__ 추가
        * __<U>Stream API</U>__
        * JVM의 __<U>Permgen의 영역 변화</U>__


      * Java SE 11
        * __<U>Oracle JDK와 OpenJDK가 통합되었으며 Oracle JDK 가 유료 모델로 전환</U>__
        
        이러한 특징이 존재합니다.
<br>

