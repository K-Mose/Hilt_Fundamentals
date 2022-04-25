# Hilt_Fundamentals
<a href="https://www.udemy.com/course/android-architecture-componentsmvvm-with-dagger-retrofit/">
Complete Android Jetpack Bootcamp(With Jetpack Compose)2022</a> - Dagger Hilt 


## Dagger & Hilt
Dagger는 안드로이드 앱 개발에서 DI(Dependency Injection)를 서포트하기 위해 개발된 라이브러리입니다. 
Hilt는 Dagger로 DI를 좀 더 쉽게 적용하기 위해 새롭게 생성된 라이브러리 입니다. <br>
https://dagger.dev/hilt/ 에서는 Hilt의 목적을 아래와 같이 정의합니다. 
- To simplify Dagger-related infrastructure for Android apps.
- To create a standard set of components and scopes to ease setup, readability/understanding, and code sharing between apps.
- To provide an easy way to provision different bindings to various build types (e.g. testing, debug, or release).

Android Developer에서는 HIlt를 아래와 같이 서술합니다. 
<p>Hilt is built on top of the popular DI library
<a href="/training/dependency-injection/dagger-basics">Dagger</a> to benefit from the
compile-time correctness, runtime performance, scalability, and <a href="https://medium.com/androiddevelopers/dagger-navigation-support-in-android-studio-49aa5d149ec9">Android Studio
support</a>
that Dagger provides. For more information, see <a href="#hilt-and-dagger">Hilt and
Dagger</a>.</p>

Hilt는 Dagger위에 작성되었기 때문에 Dagger를 이용하며, Dagger를 쉽고 효율적으로 사용할 수 있도록 합니다. 
그렇기 떄문에 Hilt를 공부하기 앞서서 Dagger를 이해하는 것이 우선입니다. 
