# JVM의 구조, JVM동작 과정과 JAVA의 실행과정

## 목차 
## 1. [JVM이란](#1-jvmjava-virtual-machine이란)

## 2. [Java의 실행과정](#2-java의-실행-과정)

## 3. [JVM의 구조](#3jvm의-구조)

## 4. [JVM의 동작과정](#4jvm의-동작과정)



<br>

---

## 1. JVM(Java Virtual Machine)이란 
<img width="500" alt="스크린샷 2023-03-20 오전 11 04 31" src="https://user-images.githubusercontent.com/81874493/226295561-605f636a-f63a-4112-9ea5-d667d861db81.png">

<br>

 * 정의 : JVM은 자바 프로그램을(컴파일된 코드) 실행시키는 가상의 컴퓨터 입니다.
    
    ⇒ OS에 종속받지 않도록 OS위에서 Java를 실행 시키는 가상의 컴퓨터

<br>

* JVM의 역할
    * 자바 애플리케이션을 읽어들여 자바API와 함께 실행
    * Java와 OS 사이의 중개자 역할을 수행하여 OS에 종속받지 않고 독립적 실행을 가능케 한다.
    * Garbage Collector를 통해 Garbage Collection 수행

<br>

* JVM의 특징
    * 스택 기반의 가상머신 이다.
    
        가상 머신의 구현체는 명세서를 어떻게 구현하는가에 따라 여러 종류로 구분 된다

        <br>

        <details>
            <summary>[가상머신이 되기 위한 필요 조건]</summary>

        * 소스코드를 VM이 실행 가능한 바이트 코드로 변환이 가능 
        
            (명령어와 피연산자를 포함하는 데이터 구조를 포함 해야 한다) 
        
        <br>

        * 함수를 실행하기 위한 콜 스택이 존재해야 한다
        
        <br>
        
        * IP(Instruction Pointer) 존재
            * 마이크로프로세서(중앙 처리 장치) 내부에 있는 레지스터 중의 하나로서, 다음에 실행될 명령어의 주소를 가지고 있어 실행할 기계어 코드의 위치를 지정한다. ( = 프로그램 카운터(Program counter, PC))

        <br>

        * 가상 CPU
            * 다음 명령어를 패치 & 명령어를 해석 & 명령을 실행

        </details>

       
        
        >⇒ 위의 조건을 만족하는 가상머신을 구현하는 방법으로 `Stack 기반 가상머신`, `register 기반 가상머신`이 존재한다

        <br>

        <details>
            <summary>Stack 기반 가상머신</summary>

        * JVM이 Stack 기반 가상머신에 포함

        * 피연산자와 연산 후 결과를 스택에 저장한다.

        
        * 아래의 이미지와 같이 더하기 연산을 할 경우 Stack 구조이기 때문에 PUSH & POP 명령이 필요하다
            
            <br>
    
            <img width="500" alt="스크린샷 2023-03-20 오전 11 04 31" src="https://user-images.githubusercontent.com/81874493/229106854-91589b4d-8ff9-444c-974b-f304276ae70e.PNG">

        <br>

        * 장점

            * 하드웨어에 덜 의존적이다
            
                ⇒하드웨어(레지스터, CPU)에 대해 직접적으로 다루지 않기 때문에 다양한 하드웨어에서 쉽게 VM을 구현할 수 있다

            * 다음 피연산자의 메모리 위치를 기억할 필요가 없다.

                ⇒ SP(stack pointer)가 다음 피연산자의 위치를 나타낸다. 스택에서 POP을  하면 다음 피연산자가 나오기 때문에 피연산자의 메모리를 따로 기억할 필요가 없다.

            <br>
    
        * 단점
            * 명령어의 수가 많아진다
            * 스택을 사용하는 오버헤드가 존재한다
            * 명령어 최적화를 할 수 없다
            
        </details>

        <details>
            <summary>Register 기반 가상머신</summary>

        * Lua VM, Dalvik VM이 Register 기반 가상머신에 포함
        * 피연산자가 CPU의 레지스터에 저장된다
        * 피연산자를 레지스터에서 가져와서 계산한 뒤 다시 레지스터에 저장한다

        <br>
        
        * 장점
            * 명령어의 수가 적다 (PUSH&POP 명령없이 하나의 명령어로 계산이 가능히기 때문)
            * 스택을 사용하지 않아 스택에 대한 오버헤드가 없다
            * 명령어 최적화가 가능하다
                * 코드에 동일한 연산이 존재할때 처음 계산한 결과를 레지스터에 넣어 여러번 사용이 가능하다. (계산 비용 최적화)
        
        <br>

        * 단점
            * 명령어의 크기가 커진다
                ( 명령어에 피연산자의 메모리 주소를 작성해야 하기 때문에 길어진다.)
        </details>
        
         <br>

    * 심볼릭 레퍼런스 (Symbolic Reference)  

        기본 자료형을 제외한 모든 타입을 명시적인 메모리 주소 기반의 레퍼런스가 아닌 <U>**심볼릭 레퍼런스**</U>를 통해 참조한다.
        <details>
        <summary>Symbolic Reference란?</summary>

        자바는 동적 링킹(Dynamic Linking)을 사용하여 실행 가능한 파일을 만들 때 프로그램에서 사용하는 모든 라이브러리 모듈을
        
        >복사하지 않고, 해당 모듈의 주소만 가지고 있다가 런타임에 실행 파일과 라이브러리가 메모리에 위치될 때 해당 모듈의 주소로 가서 필요한 것을 들고 오는 방식을 사용합니다. 

        자바가 동적 링킹(Dynamic Linking)을 사용할 수 있는 이유는
        
        >.class 파일이 실행 가능한 형태가 아닌 JVM이 읽을 수 있는 형태 Java Byte Code 이기 때문입니다. class 파일은 JVM위에서 Linking 작업을 수행할 수 있도록, 라이브러리에 대한 Symbolic Reference만을 가지고 있게 됩니다.

        </details> 
    
    <br>
    

    * Garbage Collection(가비지 컬렉션)
        * 클래스의 인스턴스는 사용자 코드에 의해 명시적으로 생성되며 더 이상 참조하지 않는다면 GC(Garbage Collector)에 의해 자동으로 메모리를 반환한다.
    
    <br>

    * 플랫폼의 독립성 보장
        * C/C++등의 언어는 플랫폼에 따라 int형의 크기가 변한다

        >JVM은 기본 자료형을 명확히 정의해 호환성을 유지하며 플랫폼의 독립성을 보장한다.

    <br>
        
    * 네트워크 바이트 오더(network byte order)
      * 자바 클래스 파일은 네트워크 바이트 오더를 사용한다.
        > 인텔 x86 아키텍처가 사용하는 리틀 엔디안이나, RISC 계열 아키텍처가 주로 사용하는 빅 엔디안 사이에서 <U>**플랫폼 독립성을 유지하려면 고정된 바이트 오더를 유지해야 하므로 네트워크 전송 시에 사용하는 바이트 오더인 네트워크 바이트 오더를 사용한다**</U>. 네트워크 바이트 오더는 빅 엔디안이다.
        
      * 네트워크 바이트 오더란?
          - 네트워크 상 데이터 전송시 연속되는 바이트를 저장하는데 사용되는 순서 = 바이트 저장 순서(Byte order)에 사용하는 규칙
          
          -  데이터 저장 방식에 따라 두가지로 구분
             - 빅 엔디안(Big Endian)
             - 리틀 엔디안(Little Endian)
            
        <br>

          - 네트워크 바이트 오더는 빅 엔디안이 가장 흔한 포멧
            - TCP, UDP, IPv4, IPv6과 같은 프로토콜은 빅 엔디안 방식 사용
            - 소켓 통신시 자주 사용되는 읽기/쓰기 함수들 경우 내부적으로 엔디안 변환 처리가 다 되어 있다. 
            
<br>


---
## 2. Java의 실행 과정


<img width="500" alt="스크린샷 2023-03-20 오전 11 04 31" src="https://user-images.githubusercontent.com/81874493/231426896-450f565e-2c12-4f6d-b6d2-ca8b7f9f6138.png">


1. 작성한 자바 소스(java source), 즉 확장자가 .java인 파일을 자바 컴파일러(Java Compiler)를 통해 자바 바이트 코드(Java Byte Code)로 컴파일 한다.
<br>

1. 컴파일된 바이트 코드를 JVM 클래스 로더(Class Loader)에게 전달한다.
<br>

1. 클래스 로더는 동적 로딩(Dynamic Loading)을 통해 필요한 클래스들을 로딩 및 링크하여 런타임 데이터 영역(Runtime Data area), 즉 JVM 메모리에 올린다.

<br>

4. 실행 엔진(Execution Egine)은 JVM 메모리에 올라온 바이트 코드들을 명령어 단위로 하나씩 가져와 실행한다.


<br>

---

## 3.JVM의 구조
    
<img width="500" alt="스크린샷 2023-03-20 오전 11 04 31" src="https://user-images.githubusercontent.com/81874493/231420535-f0aa6561-873c-4404-bd73-d3d10f58bcff.png">

- [클래스 로더](#클래스-로더-class-loader)
- [런타임 데이터 영역](#런타임-데이터-영역-runtime-data-areas)
- [실행 엔진](#실행-엔진execution-engine)

<br>

### 클래스 로더 (Class Loader)
- 클래스 로더란
  - .class' 바이트 코드를 읽어 들여 class 객체를 생성하는 역할을 담당한다.

     ⇒ 클래스가 요청될 때 class파일로부터 바이트 코드를 읽어 메모리로 로딩하는 역할

    자바 클래스들은 한 번에 모든 클래스가 메모리에 올라가지 않는다.

    각 클래스들은 필요할 때 애플리케이션에 올라가게 되며, 이 작업을 클래스로더가 해주게 된다 (동적로딩).
    
    <br>

- 클래스 로더의 특징
    <br>

  - 계층구조

    <img width="500" alt="스크린샷 2022-01-10 오후 5 55 44" src="https://user-images.githubusercontent.com/81874493/231444133-acd37e18-3ddc-4042-a241-39736a865f78.png">
    <br>
    (클래스 로더는 단순히 하나로 이루어져 있지 으며 위의 그림 처럼 여러 클래스 로더 끼리 부모-자식 관계를 이루어 계층적 구조로 되어있다.)

    - Bootstrap Class Loader
      - 최상위 클래스로더로 유일하게 JAVA가 아니라 네이티브 코드로 구현되어 있다.
      - JVM이 실행될때 같이 메모리에 올라간다.
      - Object 클래스를 비롯 Java API들을 로드한다.

        <br>

    - Extension Class Loader
      - JRE의 확장 클래스를 로드하고, 확장 클래스 패스를 사용하여 클래스를 검색하는 역할
      
        (JRE의 lib/ext 디렉토리에서 클래스를 로드)

      -  JRE에 포함된 JDBC 드라이버와 같은 확장 클래스들은 Extension Class Loader를 사용하여 로드
      
      <br>  

    - System Class Loader
      - 애플리케이션 클래스를 로드하고, 애플리케이션 클래스 패스를 사용하여 클래스를 검색하는 역할을 한다.
      
        (Java Classpath의 경로를 통해 클래스를 검색하고 로드)
        
    
    

    <br>

    - User-Defined Class Loader
        
      - 사용자 정의 클래스 로더
      - 사용자가 직접 작성한 클래스를 로드하고, 클래스 로딩 프로세스를 커스터마이징할 수 있는 역할을 한다.
      -  사용자 정의 가능한 특성이기 때문에, 애플리케이션 개발자들은 특정 클래스 로딩 동작을 제어하기 위해 User-Defined Class Loader를 구현하여 사용할 수 있다.
      

    <br>
    <br>
<img width="624" >

  - Delegation 원칙
  
    <img width="500" alt="스크린샷 2023-04-12 오후 8 25 34" src="https://user-images.githubusercontent.com/81874493/231443630-06f9013f-4cf2-4138-a6d9-88a1e9d70ea0.png">

    어떠한 클래스 파일을 로딩할 때, 해당 로딩 요청이 부모 클래스 로더들로 거슬러 올라가 BootstrapClassLoader(최상위 ClassLoader)에 다다른 후 그 밑으로 로딩 요청을 수행
  
    <로드 과정>

    1. 클래스 로더 캐시
    
        (이전 자신이 로드한 클래스인지 클래스 로더 캐시를 확인)

    2. 상위 클래스 로더
  
        (클래스 로더 캐시에 없으면 상위 클래스 로더에게 로딩을 위임)
    
        (이때 올라가는 도중 클래스를 발견하더라도 부트 스트랩 클래스 로더까지 꼭 확인해서 부트 스트랩 클래스 로더에 해당 클래스가 존재하면 부트스트랩 클래스 로더에 있는 클래스를 로드한다.)

    3. 자기 자신
    
        (만약 부트스트랩 클래스 로더에도 해당 클래스가 없으면 로드를 요청 받은 클래스 로더가 파일 시스템에서 해당 클래스를 찾는것으로 마무리한다)
    
        (최종적으로 클래스를 찾을 수 없다면 ClassNotFoundException을 발생)

    <br>

    <Delegation원칙을 사용하는 이유>
    
    * 안정성과 보안성
       * Delegation 원칙은 클래스 로딩 시 보안성과 안정성을 보장한다.
       
         이는 부모 클래스 로더에 의해 이미 로드된 클래스가 다시 로드되지 않으므로, 같은 클래스가 서로 다른 버전으로 로드되는 것을 방지한다.
          
            또한 부모 클래스 로더에 의해 로드된 클래스가 메모리에 유지되어 재사용되므로, 중복되는 클래스 로딩을 방지하여 애플리케이션의 성능을 향상시킨다.

     * 모듈화 
       * Delegation 원칙을 사용하면 클래스 로딩이 계층 구조로 구성되므로, 다른 모듈 간의 의존성 관계를 나타내기 쉽다.
        
            이를 통해 모듈 간에 종속성이 줄어들고, 각 모듈이 독립적으로 작동할 수 있도록 만들어 준다.

     * 유연성 
       * Delegation 원칙을 사용하면 클래스 로더의 구현을 변경하여 사용자 정의 클래스 로더를 구현할 수 있다. 이를 통해 애플리케이션 개발자들은 애플리케이션의 특정 요구 사항에 따라 클래스 로딩 프로세스를 커스터마이징할 수 있다.

  <br>


  - 가시성 제한(Visibility)
    
    클래스 로더가 클래스 로드를 요청받았을때 위임 모델에 의해 클래스 로더 캐시를 확인하고 없으면 상위 클래스 로더를 확인하는데
    
    ⇒ 이때 하위 클래스 로더에 있는 클래스는 확인이 불가능한 특성이 바로 가시성 제한이다.
    
    <br>

    <가시성 제한을 사용하는 이유 >
    * 클래스 로더의 계층 구조에서 하위 클래스 로더에서 상위 클래스 로더에게는 접근이 가능하지만, 상위 클래스 로더에서 하위 클래스 로더로의 접근은 불가능 하다. 
     
        이를 통해, 하위 클래스 로더가 상위 클래스 로더의 클래스를 재정의하지 못하도록 보호할 수 있다.

    * 하위 클래스 로더가 상위 클래스 로더의 클래스를 참조할 수 있도록 하지만, 상위 클래스 로더는 하위 클래스 로더의 클래스를 액세스할 수 없도록 하는 것이 가능하다.

    * 클래스 로더 간에 서로 독립적으로 작동할 수 있도록 하는 것입니다. 이 경우, 각 클래스 로더에서 로드한 클래스는 다른 클래스 로더에서 접근할 수 없다.

    이러한 가시성 제한을 통해, 클래스 로더는 클래스를 로드하고 액세스할 때의 안정성과 보안성을 유지할 수 있다.

    <br>    

  - 언로드 불가 (Unload Impossibility)
    
    * 클래스를 로드하는 것은 가능하지만 반대로 언로드 하는것은 불가능하다는 특성
    
        클래스 로더가 로드한 클래스를 메모리에서 제거하지 않는 이유는, 클래스 로딩 시 발생하는 많은 복잡한 문제를 방지하기 위해서 이다
        
        (ex 클래스가 한 번 로드된 후 다른 클래스에서 이를 참조하고 있을 때, 해당 클래스를 메모리에서 제거하면 참조하는 클래스에서 NullPointerException이 발생할 수 있다.)
    
    * 단, Java에서는 클래스 로더를 제거하면 해당 클래스 로더가 로드한 클래스도 모두 제거 된다. 이는 일반적으로 애플리케이션이나 서버가 종료될 때 발생
    
    * 일반적으로 메모리관리는 JVM의 GC(Garbage Collection) 작업을 수행하여 더 이상 참조되지 않는 객체를 자동으로 메모리에서 제거 한다.
    
    <br>

  - 이름 공간(Name Space)
    
    네임 스페이스는 각 클래스 로더들이 가지고 있는 공간으로써 로드된 클래스를 보관하는 공간으로 클래스 로드할 때 위임 모델을 통해 상위 클래스 로더들을 확인하는데 그 때 확인하는 공간이 바로 네임 스페이스 이다.
        
    * 클래스 로더 간의 클래스 이름 충돌을 방지하기 위해 필요
    * 서로 다른 클래스 로더의 네임스페이스에서 로딩될 경우 서로 다른 클래스로 인식
  
  <br>

  - 클래스 로딩 과정
  
    <img width="500" alt="스크린샷 2023-04-12 오후 8 25 34" src="https://user-images.githubusercontent.com/81874493/231728302-21daec60-086d-4928-aa02-c75738c6ee71.png">
    
    소스코드 컴파일 → 클래스 파일 → <U>**클래스 파일을 JVM이 실행 가능한 형태로 메모리에 로드(로딩 과정)**</U> 
    

    <br>

    위의 로딩 과정은 3가지 단계로 나뉜다
    * [로딩 (Loading)](#로딩-loading)
    * [링킹 (Linking)](#링킹-linking)
    * [초기화 (Initializing)](#초기화initializing)

    <br>    

    ### 로딩 (Loading)
    .class 파일을 읽어 내용에 따라 적절한 바이너리 데이터를 생성하고, 메서드 영역에 저장
      - 메서드 영역: 추후에 jvm의 구성 중, 데이터 영역에서 다룰 내용으로 자세한 설명은 생략  Type정보(class, interface, enum) Method, 변수, FQCN(클래스가 속한 패키지명을 모두 포함한 이름)이 저장되는 영역
    
    <br>
    
    앞에서 설명한, ClassLoader의 <U>**계층구조 및 Delegation원칙에 따라서 Root ClassLoader에서부터 load가 필요한 class를 찾는다**</U>

     로딩이 끝나면, Type 정보로 저장된 Class Type Object를 생성하여 Heap 영역에 저장

    <details>
        
      <summary> 클래스 로드 타임(Class Load Time) </summary>
      
      <br>

      ClassLoader에서 <U>**Class를 Load 하는 시점**</U>에 따라
      * Load-Time Dynamic Loading 
      * Run-Time Dynamic Loading
      
        으로 구분된다

      <br>

      * Load-Time Dynamic Loading
        * 하나의 Class를 Loading 하는 과정에서 이와 관련된 Class들을 한꺼번에 Loading 한다. 
        
        ```java
        public class Test { 
          public static void main(String[] args) { 
            System.out.println("hi");  
          }  
        }
        ```

        Test라는 Class에서 String객체를 Parameter로 사용하고 있으며 System객체를 호출하고 있다 이 경우 Hello Class가 ClassLoader에 의해 JVM내로 Loading 될 때 `java.lang.String Class`와 `java.lang.System Class`가 동시에 Loading이 이루어진다.

      <br>
      <br>    

      * Run-Time Dynamic Loading

        ```java
        public class Test { 
          public static void main(String[] args) { 
            System.out.println("hi");  
          }  
        }
        ```
        객체를 참조하는 순간에 동적으로 Loading 하는 방식이다

        `Class.forName()`이 실행되기 전까지는 Hello클래스에서 어떤 클래스를 참조하는지 알 수 없다

        Hello클래스의 main() 메서드가 실행되고 `Class.forName(args[0])`을 호출하는 순간에 args [0]에 해당하는 클래스를 읽어온다

        즉, 클래스를 로딩할 때가 아닌 코드를 실행하는 순간에 클래스를 로딩하는 것.
      
      

    </details>


    <br>
    <br>    

    ### 링킹 (Linking)
    로딩 단계로부터 생성된 실행될 수 있는 클래스와 리소스를 JVM의 런타임 데이터로 합치는 과정
    - Verifying
      - .class 파일의 정확성을 보장하기 위한 단계
      - 파일이 적절한 포맷인지, 유효한 컴파일러에 의해 생성되었는지를 확인
      - 검증이 실패한 경우 런타임 에러 (java.lang.VerifyError) 발생
    
    <br>

    - Preparing
      - Prepare 단계에서는 클래스 변수(static 변수)가 JVM의 메모리에 할당된다.
      - 이 단계에서는 클래스 변수가 사용될 메모리 공간이 할당되고, 변수의 기본값이 설정되며 기본값에 필요한 메모리를 준비하는 과정
    
    <br>
    
    - Resolving
      - 심볼릭 메모리 레퍼런스를 메모리 영역에 존재하는 실제 레퍼런스로 교체
      - optional 한 단계 (설정에 따라서 동작 유무가 정해진다.)

    <br>
    <br>    

    ### 초기화(Initializing)
    - 객체의 필드 값을 초기화하고, 생성자를 호출하여 객체를 초기화하는 작업이 이루어 진다.
    - 윗 단계인 Linking의 Preparing 단계에서 확보한 메모리 영역에 static 값을 할당
    - 필드의 초기값이 명시되지 않으면, JVM은 해당 필드 타입에 맞게 기본값을 할당


<br>
<br>

### 런타임 데이터 영역 (Runtime Data Areas)

JVM은 Java **컴파일러**가 컴파일한 ByteCode를 **ClassLoader**를 이용해 메모리(**RuntimeDataArea**)에 실행 가능한 상태로 적재한다.

⇒ RuntimeDataArea는 JVM이 프로그램을 수행하기 위해 OS로부터 별도로 할당받은 메모리 영역이다.

<br>


<img width="500" alt="스크린샷 2023-04-12 오후 8 25 34" src="https://user-images.githubusercontent.com/81874493/231745134-fda0f70a-b7a2-46a6-b134-826d717e3293.png">
Runtime Data Area는 

* [PC Registers](#pc-register) 
* [JVM Stacks](#jvm-stacks)
* [Native Method Stacks](#native-method-area)


<br>

* [Heap](#heap)
* [Method Area](#method-area)

    으로 구성된다
    
    <br>

    ⇒ 이 중 <U>**PC Register**</U>, <U>**JVM Stack**</U>,  <U>**Native Method Stack**</U>은 <U>`각 스레드 별로 존재 즉 쓰레드마다 서로 다른 메모리 공간이 할당된다`</U>
    
    <br>

    ⇒ <U>**Heap**</U> 과 <U>**Native Method Stacks**</U>은 <U>`각각의 스레드가 모두 공유하여 사용한다.`</U>

  <br>
  <br>

  ### PC Register

  - 현재 수행 중인 JVM 명령의 주소
  - Thread가 생성될 때마다 생기는 공간으로 Thread가 어떠한 명령을 실행하게 될지에 대한 부분을 기록
  - JVM은 Stacks-Base 방식으로 작동하는데, JVM은 CPU에 직접 명령을 전달하여 수행하지 않고, Stack에서 Operand(연산을 수행하기 위해 필요한 입력값)를 뽑아내 이를 PC Register에 저장하여 CPU가 명령을 실행하도록 한다 


  <br>
  <br>

  ### JVM Stack
  - 각각 스레드가 시작될 때 생성
  - JVM Stack에는 메서드 호출 시 생성되는 스택 프레임(Stack Frame)이 저장된다.
    
    (스택 프레임은 메서드 호출 시에 해당 메서드에서 사용하는 지역 변수, 연산 중에 필요한 값, 메서드 수행 후 반환할 주소 등의 정보를 저장)

  - 메서드가 수행될 때마다 하나의 스택 프레임이 생성되어 해당 스레드의 JVM stack에 추가 되고 메소드가 종료되면 스택 프레임이 제거
      
  - Stack Frame 구성 정보
    - Local Variable Array
      -  해당 메서드에서 사용되는 지역 변수들을 저장하는 배열으로 지역 변수는 메서드 내부에서만 사용되며, 다른 메서드에서는 접근할 수 없다
      
          Local Variable Array는 메서드 내부에서 선언된 지역 변수들을 저장하며, 인덱스를 이용해 각 변수에 접근할 수 있다.

    <br>

    - Operand Stack
      -  해당 메서드에서 수행하는 연산에 필요한 피연산자들을 저장하는 스택이다. 
      
          연산을 수행할 때, Operand Stack에서 필요한 피연산자를 가져와 연산을 수행하고, 그 결과를 다시 Operand Stack에 저장한다.

    - Constant Pool의 레퍼런스
      -  해당 메서드에서 사용되는 상수들을 저장하는 Constant Pool의 레퍼런스를 가진다.
      
          Constant Pool은 클래스 파일 내부에 저장된 상수들을 담고 있는 테이블로, 해당 메서드에서 사용되는 상수들은 모두 Constant Pool에서 가져와 사용 한다.




  <br>
  <br>

  ### Native Method Stack
  - 자바 외의 언어로 작성된 네이티브 코드를 위한 스택
  - Java Native Interface를 통해 호출하는 C/C++ 코드를 수행하기 위한 스택

  <br>

  >JVM은 Java 언어로 작성된 메서드를 해석하고 실행하는 데 최적화되어 있지만, native method는 직접 CPU 명령어로 작성되어 있어 JVM에서 해석할 수 없다.

  >따라서, JVM은 native method를 호출할 때 native method stack을 생성하고, 해당 스택에서 native method를 실행 한다.

  <br>

  - Native Method란
    - Java 언어는 자체적으로 파일 입출력, 그래픽 처리, 수학 연산 등을 지원하기는 하지만, 이들 기능을 더욱 효율적으로 처리하기 위해 C, C++, Assembly 등으로 작성된 함수를 사용하기도 한다.
    
      이러한 함수를 native method라고 한다.


  <br>
  <br>

  ### Heap
  - JVM이 관리하는 프로그램 상에서 데이터를 저장하기 위해 런타임 시 동적으로 할당하여 사용하는 영역
  - new 연산자로 생성된 객체 또는 인스턴스와 배열을 저장
  - 힙 영역에서 생성된 객체와 배열은 스택 영역의 변수나 다른 객체의 필드에서 참조
  - 참조하는 변수나 필드가 없다면 의미 없는 객체가 되어 GC의 대상이 된다
  - Heap과 Method Area는 각각의 스레드가 메모리를 공유한다.
  - Heap의 구조는 JVM을 구현한 vender마다 다르다.

  <br>

  <Oracle의 Hotspot JVM Heap구조>

  <img width="500" alt="스크린샷 2023-04-12 오후 8 25 34" src="https://user-images.githubusercontent.com/81874493/232293484-12f2b8e4-cd4f-4990-a176-555edf539b89.png">

  GC가 발생하는 영역이며, 참조(레퍼런스)가 없는 객체들은 GC과정을 통해 메모리에서 제거된다.

  Heap 영역 또한 내부적으로 여러 영역으로 나뉘어 있으며, 이는 객체의 lifecycle 및 GC와 연관이 있다.

  <br>

  **Young Generation** : (Eden 영역과 Survivor영역)

  - Eden 영역
      - Object(객체)가 최초로 Heap에 할당되는 장소
      - 만일 Eden 영역이 가득 찼다면, Object의 참조 여부를 파악하고 LiveObject는 Suvrvior 영역으로 넘긴다
      - 모든 LiveObject가 Survivor영역으로 넘어간다면 Eden영역을 청소 (참조가 사라진 GarbageObject)
      <br>
  - Survivor 영역
      - Survivor1 과 Survivor2로 구성
      - Eden영역에 살아남은 Object들이 잠시 머무르는 곳이며 LiveObject들은 하나의 Survivor 영역만 사용
      - 이러한 전반적인 과정을 Minor GC라고 한다.

  <br>

  **Old Generation**

  - Survivor1 또는 Survivor2 영역을 왔다 갔다 하는 과정에서 끝까지 살아남은 객체만이 Old 영역으로 이동
  - 보통 Old 영역은 Young 영역보다 크게 할당하며, 이러한 이유로 Old 영역의 GC는 Young 영역보다 적게 발생
  - Old Generation의 메모리가 충분하지 않으면 해당 영역에서 GC가 발생하는데 이를 Major GC라고 한다

  <br>

  **Permanent Generation**

  - Class의 Meta정보나 Method의 Meta정보, Static변수와 상수 정보들이 저장되는 공간, 흔히 메타데이터 저장 영역
  - 객체의 생명 주기가 길다고 판단되는 객체들을 이 영역에 할당하여 GC대상에서 제외를 하기 위해서 만들어진 영역
  - 주로 자바의 Class 객체들이나 문자열에 속한 String 객체들이 위치

  <br>
  <br>

  <Permanent 영역의 문제점>
  - 클래스 로더의 계층 구조가 복잡한 애플리케이션에서는 permanent generation 영역의 관리가 어렵고 복잡해지는 문제
  - Permanent Gen이 부족할 경우 `java.lang.OutOfMemoryError : permGen space` 에러가 남

  <br>

  에러 요인
  - static Object의 잘못된 사용
  - Class, Method Meta data의 증가

    ⇒ 이러한 문제를 해결 위해 <U>**Java 8 버전부터 Permanent Generation이 존재하지 않고 해당 영역을 MetaSpace로 대체**</U>

    <br>

    <img width="500" alt="스크린샷 2023-04-12 오후 8 25 34" src="https://user-images.githubusercontent.com/81874493/226532935-a3e76430-61b9-49a2-864a-dae0336f9706.PNG">

  - Static Object 및 상수화된 String Object를 heap 영역으로 옮김으로써, 최대한 GC가 될 수 있도록 수정
  - Metaspace 영역은 Heap이 아닌 Native 메모리 영역으로 취급(Heap 영역은 JVM에 의해 관리된 영역이며, Native 메모리는 OS 레벨에서 관리하는 영역으로 구분)
    - 힙 영역이 아닌 "native 메모리"를 사용하며, 따라서 JVM이 사용할 수 있는 메모리 양이 증가 
    - 메모리 할당 및 해제를 관리하는 방식이 달라졌기 때문에 permanent generation에서 발생하던 문제들이 크게 개선

  <br>

  <br>
  <br>

  ### Method Area

  <img width="500" alt="스크린샷 2023-04-12 오후 8 25 34" src="https://user-images.githubusercontent.com/81874493/232293576-67834439-325d-4dd0-b5eb-e34f502151cb.png">

  프로그램 실행 중 클래스가 사용되면 JVM은 해당 클래스 파일을 읽어서 분석하여 클래스의 인스턴스 변수, 메서드 코드 등을 Method Area에 저장한다

  - Type information
      - Type은 클래스와 인터페이스를 통칭하는 것으로 이해하면 된다
      - 클래스와 관련한 모든 정보
      
      <br>

  - Constant Pool
      - 문자열 상수와 같은 리터럴 상수
      - 메서드와 필드에 대한 모든 Reference를 담고 있음 (Symbolic Reference) 즉, 어떤 메서드나 필드를 참조할 때, JVM은 런타임 상수 풀을 통해 메소드나 필드의 실제 메모리 상 주소를 참조하여 중복을 막는 역할
      
      <br>

  - Field Information
      - Class 멤버 변수의 이름 및 데이터 타입, 접근 제어자에 대한 정보를 저장
      
      <br>

  - Method Information
      - Class 멤버 메서드의 이름, 리턴 타입, 매개변수, 접근제어자에 대한 정보
      
      <br>

  - Class Variable
      - static으로 선언되는 모든 클래스 변수
      - 이 변수는 모든 Instance에서 접근 가능하기 때문에 동기화 이슈가 발생할 수 있음 Class Variable을 final로 선언할 경우에는 Constant Pool에 저장
    
      <br>

  - Reference to ClassLoader & class Class
      - 특정 클래스를 로드한 클래스로더의 정보를 관리
      - Class object와 서로 양방향 접근을 하기 대문에 Class Object에 대한 참조 주소 값을 가진다
      
      <br>

  - Method Table
      - Class의 Method에 대한 Direct Reference를 가진다고 보면 된다
      - Method Table을 이용해 Super Class에서 상속된 Method의 Reference까지 확인이 가능

<br>
<br>

### 실행 엔진(Execution Engine)
- 실행 엔진은 클래스 로더를 통해 런타임 데이터 영역에 배치된 바이트 코드를 명령어 단위로 읽어 실행한다
- 바이트 코드의 각 명령어는 1바이트 크기의 OpCode(Operation Code)와 추가 피연산자로 구성되어 있다.
- 실행 엔진은 하나의 OpCode를 가져와 피연산자와 작업 수행하고 다음 OpCode를 수행하는 식으로 동작

<br>

자바는 기본적으로 컴파일러를 사용하여 소스 코드를 바이트 코드로 변환하고 바이트 코드는 JVM에서 실행된다.

따라서, 자바는 처음 한번만 컴파일러를 사용하여 소스 코드를 컴파일하여 바이트 코드를 생성하며, 이후에는 생성된 바이트 코드를 실행할 때 인터프리터나 JIT 컴파일러를 사용한다.

<br>

  - JIT 컴파일러(Just-In-Time Compiler) : 인터프리터의 단점을 보완하기 위해 도입된 방식으로 바이트 코드 전체를 컴파일하여 바이너리 코드로 변경하고 이후에는 해당 메서드를 더이상 인터프리팅 하지 않고, 바이너리 코드로 직접 실행하는 방식이다.
  
    하나씩 인터프리팅하여 실행하는 것이 아니라 바이트 코드 전체가 컴파일된 바이너리 코드를 실행하는 것이기 때문에 전체적인 실행속도는 인터프리팅 방식보다 빠릅니다.
  
  <br>

  - 자바 인터프리터 : 바이트 코드 명령어를 하나씩 읽어서 해석하고 실행합니다. 하나하나의 실행은 빠르나, 전체적인 실행 속도가 느리다는 단점을 가집니다.

⇒ 일반적으로 자바의 실행 엔진은 인터프리터와 JIT 컴파일러를 혼합하여 사용한다.

   초기에는 인터프리터를 사용하여 코드를 실행하고, 런타임 시에 JIT 컴파일러를 사용하여 반복적으로 실행되는 코드를 미리 컴파일한다.
    
  이러한 방식은 실행 속도와 메모리 사용량을 균형적으로 관리할 수 있도록 도와준다.

<br>

---


## 4.JVM의 동작과정
<img width="500" alt="스크린샷 2023-04-12 오후 8 25 34" src="https://user-images.githubusercontent.com/81874493/232293732-d1fcc8b8-4829-4ede-bfaa-7fd8811a0f41.png">

1. **작성한 자바 소스(.java)를 자바 컴파일러를 통해 자바 바이트 코드(.class)로 컴파일 한다.**
    - 자바 바이트 코드 : JVM이 이해할 수 있는 코드로 아직 컴퓨터는 읽을 수 없는 반기계어 이다. 자바 바이트 코드의 각 명령어는 1바이트 크기의 Opcode와 추가 피연산자로 이루어져 있다.

    <br>

2. **컴파일된 바이트코드를 JVM의 클래스 로더에게 전달합니다.**
    - 클래스 로더 세부 동작
        - 로드 : 클래스 파일을 가져와서 JVM의 메모리에 로드한다.
        - 검증 : 자바 언어 명세(Java Language Specification) 및 JVM 명세에 명시된 대로 구성되어 있는지 검사한다.
        - 준비 : 클래스가 필요로 하는 메모리를 할당 한다. (필드, 메서드, 인터페이스 등등)
        - 분석 : 클래스의 상수 풀 내 모든 심볼릭 레퍼런스를 다이렉트 레퍼런스로 변경 한다.
        - 초기화 : 클래스 변수들을 적절한 값으로 초기화 한다. (static 필드)
        
        <br>

3. **JVM의 클래스 로더는 동적 로딩(Dynamic Loading)을 통해 필요한 클래스들을 로딩 및 링크하여 런타임 데이터 영역(Runtime Data area), 즉 JVM의 메모리에 올린다.**

<br>

4. **실행엔진(Execution Engine)은 JVM 메모리에 올라온 바이트 코드들을 명령어 단위로 하나씩 가져와서 실행한다. 이때, 실행엔진은 2가지 방식으로 동작할 수 있다.**
    
    - 자바 인터프리터(Java Interpreter) : 바이트 코드 명령어를 하나씩 읽어서 해석하고 실행합니다. 하나 하나의 실행은 빠르나, 전체적인 실행 속도가 느리다는 단점을 가진다.
    
    <br>

    - JIT 컴파일러(Just-In-Time Compiler) : 인터프리터의 단점을 보완하기 위해 도입된 방식으로 바이트 코드 전체를 컴파일하여 바이너리 코드로 변경하고 이후에는 해당 메서드를 더이상 인터프리팅 하지 않고, 바이너리 코드로 직접 실행하는 방식이다.
    
       하나씩 인터프리팅하여 실행하는 것이 아니라 바이트 코드 전체가 컴파일된 바이너리 코드를 실행하는 것이기 때문에 전체적인 실행속도는 인터프리팅 방식보다 빠르다.

<br>

# 참고
[https://jeong-pro.tistory.com/148](https://jeong-pro.tistory.com/148) JVM참고

[https://steady-coding.tistory.com/305](https://steady-coding.tistory.com/305)  JVM참고

[https://steady-snail.tistory.com/67](https://steady-snail.tistory.com/67) JVM참고