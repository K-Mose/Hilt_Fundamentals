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

## Example 
이번에는 Dagger로 작성된 DI를 Hilt로 변경하도록 하겠습니다. 
`DataSource`라는 클래스는 
### Basic Sturcture
```
HiltDemo
└─ app
   └─ src
      └─ main 
         ├─ AndroidManifest.xml
         └─ java
            └─ com
               └─ kmose
                  └─ hiltdemo
                     ├─ DataSource.kt
                     └─ MainActivity.kt
```
기본 구조는 아래와 같습니다. `DataSource`는 외부라이브러리로서 코드 수정이 블가능한 클래스입니다. 
이 클래스를 `MainActivity`에서 DI를 하도록 하는 예 입니다. 

외부에 있는 `DataSource` 클래스를 DI 하기 위해서 Module과 Component를 생성하고 Application을 상속하는 클래스를 만들어 앱이 로드될 때 의존성을 주입하는 코드로 아래와 같이 작성합니다. 
<details>
   <summary>Dagger Code</summary>
   
### AndroidManifest
```xml
    <application
        android:name=".App"
``` 

### DataSource
```kotlin 
class DataSource {
    fun getRemoteData() {
        Log.i("MYTAG", "Data downloading ... ")
    }
}
```

### DataModule
```kotlin 
@Module
class DataModule {
    @Provides
    fun providesDataSource(): DataSource {
        return DataSource()
    }
}
```

### DataSourceComponent
```kotlin 
@Component(modules = [DataModule::class])
interface DataSourceComponent {
    fun inject(mainActivity: MainActivity)
}
```

### MainActivity
```kotlin 
class MainActivity : AppCompatActivity() {
    @Inject
    lateinit var dataSource: DataSource
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        (application as App).daggerComponent.inject(this)
        dataSource.getRemoteData()
    }
}
```

### App
```kotlin 
class App : Application() {
    lateinit var daggerComponent: DataSourceComponent
    override fun onCreate() {
        daggerComponent = DaggerDataSourceComponent.builder().build()
        super.onCreate()
    }
}
```
                 
### Dagger Structure              
```
HiltDemo
└─ app
   └─ src
      └─ main 
         ├─ AndroidManifest.xml
         └─ java
            └─ com
               └─ kmose
                  └─ hiltdemo
                     ├─ App.kt
                     ├─ DataModule.kt
                     ├─ DataSource.kt
                     ├─ DataSourceComponent.kt
                     └─ MainActivity.kt
```
</details>


## Dagger to Hilt 
우선 Hilt를 사용하기 위해 project's root의 `build.gradle`에 <a href="https://developer.android.com/training/dependency-injection/hilt-android#setup">dependency</a>를 추가합니다. 
```
    dependencies {
        …
        classpath 'com.google.dagger:hilt-android-gradle-plugin:2.38.1'
    }
```
그리고 app level `build.gradle`에 pulgin과 dependency를 추가합니다. 
```
plugins {
   …
   id 'dagger.hilt.android.plugin'
}
   ……
dependencies {
    …
    implementation "com.google.dagger:hilt-android:2.28-alpha"
    kapt "com.google.dagger:hilt-android-compiler:2.28-alpha"
}
```
   
Hilt는 Java8 기능을 사용하기 때문에 app level `build.gradle`에 아래 기능도 추가합니다. 
```
android {
    ...
    compileOptions {
        sourceCompatibility = JavaVersion.VERSION_1_8
        targetCompatibility = JavaVersion.VERSION_1_8
    }
}
```

### Hilt Application Class
우선 Application class를 Hilt로 변경합니다. 모든 Hilt의 Application class는 반드시 @HiltAndroidApp 어노테이션이 필요합니다. 
그리고 클래스 내에 코드를 작성할 필요가 없으므로 다 지웁니다. 
```kotlin
@HiltAndroidApp 
class App : Application()
```
@HiltAndroidApp 어노테이션은 app level의 의존성 컨테이너로 사용되는 기본 클래스를 앱에 포함하여 Hilt 코드를 생성하는 트리거가 됩니다. 
아래와 같은 코드를 생성합니다. 
<details>
   <summary>class Hilt_App</summary>
   
```java
/**
 * A generated base class to be extended by the @dagger.hilt.android.HiltAndroidApp annotated class. If using the Gradle plugin, this is swapped as the base class via bytecode transformation.
 */
@Generated("dagger.hilt.android.processor.internal.androidentrypoint.ApplicationGenerator")
public abstract class Hilt_App extends Application implements GeneratedComponentManager<Object> {
  private final ApplicationComponentManager componentManager = new ApplicationComponentManager(new ComponentSupplier() {
    @Override
    public Object get() {
      return DaggerApp_HiltComponents_ApplicationC.builder()
          .applicationContextModule(new ApplicationContextModule(Hilt_App.this))
          .build();
    }
  });

  protected final ApplicationComponentManager componentManager() {
    return componentManager;
  }

  @Override
  public final Object generatedComponent() {
    return componentManager().generatedComponent();
  }

  @CallSuper
  @Override
  public void onCreate() {
    // This is a known unsafe cast, but is safe in the only correct use case:
    // App extends Hilt_App
    ((App_GeneratedInjector) generatedComponent()).injectApp(UnsafeCasts.<App>unsafeCast(this));
    super.onCreate();
  }
}
```
</details>
위 `Hilt_App` 클래스 내에서 `generatedComponent()` 메소드로 Component를 생성하기 때문에 Hilt를 사용하게되면 Component interface가 필요없게 되므로 `DataSourceComponent` 인터페이스 파일을 제거합니다. 


