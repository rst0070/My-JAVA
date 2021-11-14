# Annotation 이해  
  
__질문들, 순서__
1. [annotation을 왜 사용하는가?](#link1)
2. annotation을 어떻게 정의?
3. annotation을 어떻게 적용하는가?
4. annotation 정보를 불러오는 방법
5. 더 알아볼것
  
## 1. annotation을 사용하는 이유<a name="link1"></a>
영문뜻은 `주석`이며 소스코드에 라벨을 붙이는 역할을 한다. 즉 소스의 정보를 표시하는것.  
이 정보만으로 새로운 로직을 만들어내진 않지만,
개발툴, 소스코드내부, 런타임, 컴파일시등에 이 정보를 참조할 수 있다.  
  
따라서 소스코드에 붙여진 정보를 통해 새로운 동작을 할 수 있게한다.  

예를들어  
`@Override`는 상속받은 메서드를 오버라이드하는 메소드임을 알려주고,
`@Deprecated`가 붙여진 메서드등을 사용할때에는 컴파일시 경고가 뜬다.

## 2. annotation 정의 방법
```java
@Target(ElementType.[적용대상])
@Retention(RetentionPolicy.[유지시기])
public @interface AnnotationName{
    public String exampleField() default "어노태이션 적용시 값을 정하지 않을때 d_efault 값";
    ...
}
```
위와 같이 annotation을 정의할 수 있다.  
사용할때는 `@AnnotationName(exampleName = "이름입력")`과 같이 내부의 필드를 정해줄 수 있으며 정하지 않을시 `default`값으로 정해진다.  
  
* `@Target` - 해당 어노테이션이 적용될 대상을 가리킨다. `ElementType`은 enum타입이며  
`ANNOTATION_TYPE`, `CONSTRUCTOR`,`FIELD`,`LOCAL_VARIABLE`,`METHOD`,`PACKAGE`,`PARAMETER`,`TYPE`(= class, interface, annotation, enum등) 을 가진다.
* `@Retetion` - 해당 어노테이션이 언제까지 유효하게 할것인지를 경정한다.
`RetentionPolicy`는 enum타입이며
    * `RetentionPolicy.CLASS` - 클래스파일로 컴파일될때까지 유효
    * `RetentionPolicy.RUNTIME` - 런타임에도 유효. 즉, 계속 존재
    * `RetentionPolicy.SOURCE` - 소스파일에서만 유효.
## 3. annotation 적용방법
__class에 적용__
```java
@AnnotationName(...)
class Test{...}
```
__method에 적용__
```java
@AnnotationName(...)
public void method(...){...}
```
__field에 적용__
```java
@AnnotationName(...)
Object field;
```
  
## 4. annotation 정보 조회하기
소스코드의 annotation정보를 불러오기위해선 `Reflection`을 이용해야한다.  
  
__Annotation 조회 과정__
1. 객체, 클래스, 필드, 혹은 메서드의 `reflection`을 가져온다.
2. `reflection`에 정의된 메서드를 이용해 Annotation을 가져온다.
3. 해당 annotation을 이용한다.
   
__ex1) 객체의 annotation__
```java
Test t = new Test();
Class c = t.getClass();
AnnotationName an = (AnnotationName)c.getAnnotation(AnnotationName.class);
System.out.println(an.exampleField());
```

__ex2) 메서드의 annotation__
```java
import java.lang.reflect.*;
import java.lang.annotation.*;

@Retention(RetentionPolicy.RUNTIME)
@interface AnnotationName{
    public String exampleName() default "default name";
}
class TestClass{
    @AnnotationName(exampleName = "this is testMethod")
    public String testMethod(){
        return "testMethod";
    }
}

class Main{
    public static void main(String[] args) throws NoSuchMethodException{
        Method method = TestClass.class.getMethod("testMethod");
        AnnotationName an = method.getAnnotation(AnnotationName.class);
        System.out.println(an.exampleName());
    }
}
```

__ex3) Field의 annotation__
```java
import java.lang.reflect.*;
import java.lang.annotation.*;

@Target(ElementType.FIELD)
@Retention(RetentionPolicy.RUNTIME)
@interface AnnotationName{
    public String exampleName() default "default name";
}
class TestClass{
    @AnnotationName(exampleName = "this is field")
    public String field = "hi";
}

class Main{
    public static void main(String[] args) throws NoSuchFieldException{
        Field field = TestClass.class.getField("field");
        AnnotationName an = field.getAnnotation(AnnotationName.class);
        System.out.println(an.exampleName());
    }
}
```
  
## 5. 더 알아볼것.
* __reflection의 동작원리, 사용법등__