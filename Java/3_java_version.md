# Java 버전별 특징

## 목차 
## 1. [JRE?](#1-jre-java-runtime-environment)

## 2. [JDK?]()

## 3. [자바의 하위호환성](#2-자바java란-무엇-인가요)

## 4. [자바8 ~ 17 특징](#3-자바java의-특징에는-무엇이-있나요)

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


### Java11
[대표적 특징]
Oracle JDK와 OpenJDK가 통합되었으며 Oracle JDK 가 유료 모델로 전환되었습니다.

* 가장 커다란 변화는 바로 라이선스 부분이다. Java SE 11 부터 Oracle JDK 의 독점 기능이 오픈 소스 버전인 OpenJDK 에 이식된다. 이는 다시 말해 Oracle JDK 와 OpenJDK 가 완전히 동일해진다는 뜻이다.


### Java17


<br>

---

## 5. 정리본

* JRE, JDK 자바의 버전별 특징

<br>

* 자바(Java)란 무엇인가요?
    * 답변 : 자바(Java)는 __프로그래밍 언어__ 중 하나로 미국 '썬마이크로시스템즈'라는 회사에서 개발한 객체지향 언어 입니다. 

<br>

* 자바(Java)의 플렛폼 독립성에 대해 설명해 주세요
    * 답변 : 자바는 컴파일러를 통해 __바이트 코드(Byte Code)__라는 .class라는 중간 코드를 생성해내고 class파일은 JVM(Java Virtual Machine)이라는 프로그램을 통해 기계어로 번역되어 한줄씩 실행됩니다.
    이 방식을 통해 어느 환경에서나 그 환경에 맞는 JVM만 설치되어 있다면 프로그램을 실행시킬 수 있다는 플렛폼의 독립성을 가지게 됩니다.

<br>


* 객체지향 언어란 무엇인지 설명해 주세요
    * 답변 : 객체지향 언어란 컴퓨터 프로그래밍의 패러다임중의 하나로 프로그래밍에서 <U>__필요한 Data를 추상화 시켜 상태와 행위를 가진 객체를 만들고 그 객체간 유기적 상호작용을 통해 로직을 구성하는 프로그래밍 방법__</U> 입니다.

<br>

* 자바의 대표적인 특징들을 간단히 설명해주세요
  * 답변 : 자바 언어 특징으로
    * JRE가 설치된 어떤 환경에서도 자바를 실행 시킬 수 있다는 <U>__플랫폼의 독립성__</U>
    * 객체지향 언어의 특징인 <U>__캡슐화, 상속성, 다형성 을 지원__</U>
    * 콘솔 프로그램, 서버용 웹 애플리케이션, 안드로이드 앱 등 <U>__다양한 어플리케이션 개발 가능__</U>
    * 스레드 생성 및 제어와 관련된 라이브러리 API를 제공하여 <U>__멀티 스레드 구현 가능__</U>
    * JVM은 애플리케이션이 실행될 때 객체가 필요한 시점에 객체를 생성 및 사용하도록 <U>__동적로딩을 지원__</U>
    * 자바는 <U>__컴파일러를 통해 바이트 코드(.class)파일을 생성__</U>하고 <U>__JVM의 자바 인터프리터를 통해 코드를 한줄씩 실행__</U>
    * 자바는 객체 생성시 <U>__자동으로 메모리 영역을 찾아서 할당__</U>하고 <U>__사용이 완료된 객체는 GC를 실행시켜 자동으로 메모리 자원을 반환__</U>
    * 자바는 프로그램에서 사용하는 <U>__오픈소스 라이브러리의 양이 방대해__</U>  검증된 오픈소스 라이브러리를 통한 빠르고 편한 개발이 가능

<br>

* 자바의 장단점에 대해 설명해 주세요
  * 장점으로는
    * 운영체제에 독립적입니다.
    * 객체지향 언어로 캡슐화, 상속 추상화 다형성등을 지원합니다.
    * JVM의 GC를 통해 자동으로 메모리를 관리해 줍니다.
    * OpenJDK가 오픈소스로 라이브러리가 풍부하여 잘 활용한다면 짧은 개발 시간내에 안정적인 애플리케이션을 구현 할 수 있습니다.
    * 스레드 API를 제공하고 있기 때문에 운영체제에 상관없이 멀티스레드를 쉽게 구현할 수 있습니다.
    * 동적로딩(Dynamic Loading)을 지원하여 유지보수가 쉽고 빠르게 실행됩니다.


<br>

  * 단점으로는
    * 자바는 코드를 기계어로 변환하기 위한 인터프리터의 추가 작업을 위해 JVM
      을 거치기 때문에 C나 C++ 의 컴파일 단계에서 만들어지는 완전한 기계어보다 속도가 느립니다.

