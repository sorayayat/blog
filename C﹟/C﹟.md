##  개요

.NET 프레임 워크 기반의 프로그래밍 언어이다.
닷넷(.NET)이란 웹 앱, 모바일, 데스크톱 등등 오픈 소스 크로스 플랫폼 개발 환경

java의 jvm이 있다면 C#은 .NET이라는 vm이 있다


c#이라는 언어는 동일 하지만 환경에 따라 변화한다.
즉, 하나의 언어로 다양한 환경에서 사용이 가능한 크로스 플랫폼이다.
![](img/Pasted%20image%2020240626120009.png)


C#은 강타입(strong type)언어이며 이러한 점은 컴파일시 오류를 검사하여 디버깅 시간을 줄일 수 있다는 점이다.
또한 GC를 통해 메모리 관리가 이루어진다는 장점이 있다.



##  JVM(Java Virtual Machine) VS CLR(Common Language Runtime)
---

Java는 **JVM(Java Virtual Machine, 자바 가상 머신)** 지원

자바의 컴파일 과정으로는

1. 자바 소스코드 → 컴파일 → 바이트코드(.class)로 변환한다.

2. 변환된 바이트 코드는 JVM상에서 번역되어 실행된다.

​

C#은 **CLR(Common Language Runtime, 공통 중간 언어)** 지원

C#의 컴파일 과정으로는

1. NET의 경우는 .NET코드 → 컴파일 → IL로 변경된다.

2. IL은 CLR에 의해 실행 시 **네이티브 코드**로 변환되어 실행된다.

​

JVM 과 CLR은 동작 원리가 거의 흡사하다. 다만, 가장 큰 차이는 **JVM은 Java Runtime으로 Java에서만 동작**할 수 있지만,

CLR은 **공용언어 Run Time으로 C#이나 기타 Visual Basic, .NET과 같은 언어에서 사용**된다.