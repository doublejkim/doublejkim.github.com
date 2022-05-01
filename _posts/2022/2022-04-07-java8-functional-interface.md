---
title: Java8 Functional Interface
layout: single
author_profile: true
read_time: true
comments: true
share: true
related: true
categories:
- java
tag:
- java
- java8
toc: true
toc_sticky: true
toc_label: 목차
description: Java8 - Functional Interface
article_tag1: java
article_tag2: interface
article_tag3: 
article_section: java
meta_keywords: 
last_modified_at: '2022-04-07 00:50:00 +0800'
---

# 1. 함수형 인터페이스 (Functional Interface)

## 1.1. Java 에서의 Functional Interface

SAM(Single Abstract Method) 인 Interface 는 모두 Functional Interface 이다. 즉, 추상메소드를 딱 하나만 가지고 있는 인터페이스를 의미한다. (Object 클래스로부터 상속 받은 메소드는 제외한다.)

위 의 조건을 만족하는 인터페이스는 Functional Interface 로 인지되며, 람다 표현식에 적용이 가능하다.

만약 Functional Interface 임을 명시적으로 알려야 할 필요가 있을경우는 @FunctionalInterface 애노테이션을 사용한다.

```java
@FunctionalInterface
public interface Blah {
    void test();
    ...
}
```

## 1.2. Java 에서 기본 제공하는 잘 알려진 Functional Interface

### Function<T, R>

파라미터 1개, 리턴 1개 형태의 apply() 형태의 SAM 표현

- T : 입력값 타입
- R : 리턴값 타입

```java
R apply(T t);
```

compose(), andThen() 으로 조합해서 사용 가능

```java
Function<Integer, Integer> plus5 = (value) -> value + 5; 
Function<Integer, Integer> mul2 = (value) -> value * 2; 

System.out.println(plus5.compose(mul2).apply(3)); // (3 * 2) + 5 
System.out.println(plus5.andThen(mul2).apply(3)); // (3 + 5) * 2
```

### BiFunction<T, U, R>

파라미터 2개, 리턴 1개 형태의 apply() 형태의 SAM 표현

- T : 첫 번째 입력값 타입
- U : 두 번째 입력값 타입
- R : 리턴 타입

```java
R apply(T t, U t);
```

andThen 으로 Function<T, R> 조합 사용가능

```java
BiFunction<Integer, Integer, Integer> func1 = (val1, val2) -> val1 * val2;  
Function<Integer, Integer> func2 = (val1) -> val1 + 1;  

System.out.println( func1.apply(2, 3) );  // 6
System.out.println( func1.andThen(func2).apply(2, 3)); // 7
```

### Consumer< T >

파라미터 1개, 리턴값은 없는 accept() 형태의 SAM 표현.
파라미터를 전달받아 소비해버리는 이미지임

- T : 입력 값 타입 

```java
void accept(T t);
```

```java
Consumer<String> supplier = (val) -> System.out.println("val : " + val);  

supplier.accept("Jack");  // val : Jack
```

### Supplier< T >

파라미터는 없고, 리턴값만 존재하는 get() 형태의 SAM 표현
무언가를 제공해주는 이미지임

- T : 리턴 타입

```java
T get();
```

```java
import java.util.function.Supplier;  

public class Foo {  

    private String name;  

    protected Foo() {}  
    public Foo(String name) {this.name = name; }  

    public String getName() {return this.name; }  

    public static void main(String[] args) {  

        Supplier<Foo> supplier = () -> new Foo("guest");  
        Foo foo = supplier.get();  
        System.out.println(foo.getName());  // guest

    }  
}
```

### Predicate< T >

파라미터는 1개, 리턴값은 boolean. test() 형태의 SAM 표현 
무언가의 판단(true/false) 의 근거가 되는 값을 판정(test) 해 주는 이미지임.

- T : 입력 값 타입

```java
boolean test(T t)
```

```java
Predicate<Integer> pred1 = (n) -> n%2 == 0; 
Predicate<Integer> pred2 = (n) -> n<10;

System.out.println(pred1.and(pred2).test(12)); // false 
System.out.println(pred1.and(pred2).test(8)); // true 
System.out.println(pred1.test(8)); // true 
System.out.println(pred1.negate().test(8)); // false
```

> 그 외 BiFunction 에서의 모든 입,출력값 타입이 동일하다던가.. 등의 
> 경우를 위해 여러가지 Functional Interface 를 제공하고있으며 해당 내용은 아래 링크 참조

---

https://docs.oracle.com/javase/specs/jls/se8/html/jls-9.html#jls-9.8

https://docs.oracle.com/javase/8/docs/api/java/util/function/package-summary.html
