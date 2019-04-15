---
layout: post
title:  "제네릭 (Generic) (2)"
date:   2019-04-14 21:20:11
author: Dongy
categories: Java
cover:
---


## 제네릭 사용 범위

<strong>1. 클래스(생성자), 인터페이스</strong><br>
-타입을 파라미터로 가지는 클레스와 인터페이스를 제네릭 타입이라고 합니다.<br>
-클래스 또는 인터페이스 이름 뒤에 붙는 “<>” 사이에 T를 <strong>타입 파라미터</strong>라고 합니다.<br>
-클래스 레벨에서 제네릭이 설정되어있으면 static 메서드에서는 사용할 수 없습니다. (인스턴스가 만들어질 때 type parameter를 받아오기 때문입니다.)<br>
<br>
<strong>2. 메서드</strong><br>
<strong>파라미터, 리턴 타입</strong>으로 사용합니다.<br>
static, instance 메소드에 사용할 수 있습니다. (인스턴스 생성할 떄 type parameter를 받아와 타입이 설정되기 때문입니다.)<br>
<br>
아래에서 구체적인 사용 방식을 살펴보겠습니다.<br>
<br>

## 제네릭 사용 방식

### 1.Class generic type, Interface generic type
Class generic type과 Interface generic type은 사용방법이 유사합니다.<br>
<br>
아래 소스는 Class generic type 사용 예입니다.
```
//구현
class ClassGenericType<T> {
        private T t;
 
        public void set(T t) {
               this.t = t;
        }
        public T get() {
               return t;
        }
}

//사용방법
ClassGenericType<String> classGenericType = new ClassGenericType<>();
```



### 2. multi type parameter(멀티 타입 파라미터)
멀티 타입 파라미터는 2개 이상의 제네릭 타입을 정의한 것입니다. 이 경우 각 타입 파라미터를 콤마로 구분하여 사용합니다.<br>
<br>
아래 코드에서는 <T1, T2>와 같이 제너릭 타입을 두 개 선언한 멀티 타입 파라미터를 사용하였습니다.

```
interface InterfaceGenericType<T1, T2> {
	public T1 doSomething(T2 t);
        public T2 doSomething2(T1 t);
}


//InterfaceGenericType 인터페이스 구현 클래스
class InterfaceGenericTypeImpl implements InterfaceGenericType<String, Integer> {
        @Override
        public String doSomething(Integer t) {
               return null;
        }
 
        @Override
        public Integer doSomething2(String t) {
               return null;
        }
}
```



### 3.Method Generic Type
제네릭 메소드는 매개 타입과 리턴 타입으로 타입 파라미터를 갖는 메소드를 말합니다.<br>
<br>
선언 방법은 메서드의 리턴 타입 앞에 <> 기호를 추가하고 타입 파라미터를 기술한 다음,<br>
리턴 타입과 파라미터 타입으로 타입 파라미터를 사용하면 됩니다.

```
public class Util {
        public static <T> Box<T> boxing(T t) {
		Box<T> box = new Box<T>();
	        box.set(t);
		return box;
        }
}
```
<br>
호출방법<br>
리턴타입 변수 = <구체적타입> 메소드명(파라미터값); //명시적으로 구체적 타입을 지정<br>
리턴타입 변수 = 메소드명(파라미터값); 			  //매개값을 보고 컴파일러가 구체적 타입을 추정<br>
<br>
Box class의 boxing method를 호출하는 방법<br>
Box<Integer> box = <Integer>boxing(100); //타입 파라미터를 명시적으로 Integer로 지정<br>
Box<Integer> box = boxing(100);			//타입 파라미터를 컴파일러가 Integer로 추정<br>
<br>





### 4. Bounded Type Parameter (제한된 타입 파라미터)
타입 파라미터(“<>” 사이에 들어갈 Type)에 지정되는 구체적인 타입을 제한할 때 사용합니다.<br>
<br>
예를 들면, 숫자를 연산하는 제네릭 메소드의 파라미터로 <br>
Number 클레스 타입 또는 하위 클래스 타입(Byte, Short, Integer, Long, Double)의 인스턴스만 가져야 한다면,<br>
extends 키워드로 받을 수 있는 파라미터를 제한할 수 있습니다.<br>
<br>
사용방법<br>
제한된 타입 파라미터를 선언하려면, 타입 파라미터 뒤에 extends 키워드를 붙이고 상위 타입을 명시하면 됩니다.<br>
```
//public <T extends 상위타입> 리턴타입 메소드이름(매개변수, …) { … }
public <T extends Number> int compare(T t1, T t2){ //… }
```
주의할 점<br>
-extends 뒤에 올 타입은 클래스뿐만 아니라 인터페이스도 가능하지만 인터페이스라고해서 Implements를 사용하지 않습니다.<br>
-메소드의 중괄호 {} 안에서 타입 파라미터 변수로 사용 가능한 것은 extends 뒤에 올 타입의 맴버 (필드, 메소드)로 제한됩니다.<br>
하위 타입에만 있는 필드와 메소드는 사용할 수 없습니다.<br>
```
public class Util {
	public <T extends Number> int compare(T t1, T t2){
		double v1 = t1.doubleValue(); // Number의 doubleValue() 메소드 사용
		double v2 = t2.doubleValue(); // Number의 doubleValue() 메소드 사용
		return Double.compare(v1, v2); //v1과 v2를 비교하여 첫번째 매개값이 작으면 -1, 같으면 0, 크면 1을 리턴
	}
}

public class BoundedTypeParameterExample {
	public static void main(String[] args){
		//String str = Util.compare(“a”, “b”); //(x) String은 Number의 하위 클레스 타입 아님
		int result1 = Util.compare(10, 20); //int -> Integer 자동 Boxing (Integer는 Number의 하위 클레스 타입)
		int result2 = Util.compare(4.5, 3); //double -> Double 자동 Boxing (Double은 Number의 하위 클레스 타입)
	}
}
```



### 5.Wildcard Generic Type
스포츠에서 와일드 카드(wild card)는 정규규칙에 의해 통과하지 못한 선수나 팀에 자격을 주는 것을 뜻하는 말로, <br>
'아무렇게나 쓸 수 있는'이라는 의미에서 나온 말입니다.<br>
<br>
코드에서 ? 를 일반적으로 와일드 카드(wild card)라고 부릅니다.<br>
<br>
제네릭 타입에 와일드카드를 다음과 같이 세 가지 형태로 사용할 수 있습니다.<br>
제네릭타입 <?> : 모든 클레스나 인터페이스 타입이 올 수 있습니다. 내부적으로는 Object로 인식합니다.<br>
제네릭타입 <? extends 객체자료형> : 명시된 클레스나 인터페이스 타입 또는 이를 상속한 하위 타입만 올 수 있습니다.<br>
제네릭타입 <? super 객체자료형> : 명시된 클레스나 인터페이스 타입 또는 이의 상위 타입만 올 수 있습니다.<br>
객체 자료형에는 클래스 뿐만 아니라 인터페이스도 가능합니다.<br>
인터페이스라고해서 extends 대신 implement를 사용하지 않습니다.<br>
<br>
쉽게 예를 들면 <br>
A > B > C > D 순으로 상속된 class들이 있을 때,<br>
<? super B> 가 의미하는 class 는 B, A class<br>
<? extends C> 가 의미하는 class 는 C, D class<br>
<br>
제네릭 타입과 와일드 카드를 아래 소스와 같이 활용할 수 있습니다.<br>
```
WildCardGeneric.java

public class WildCardGeneric<T> {
    T wildCard;

    public void setWildCard(T wildCard){
        this.wildCard = wildCard;
    }

    public T getWildCard(){
        return wildCard;
    }
}

WildCardGeneric 클래스를 사용하는 WildCardSample.java

public class WildCardSample {
    public static void main(String[] ar){
        WildCardSample ex = new WildCardSample();
        ex.callWildCardMethod();
    }

    public void callWildCardMethod(){
        WildCardGeneric<String> wildcard = new WildCardGeneric<>();
        wildcard.setWildCard("A");
        wildcardStringMethod(wildcard); // print “A”

        WildCardGeneric<Integer> wildcard2 = new WildCardGeneric<>();
        wildcard2.setWildCard(777);
        wildcardStringMethod2(wildcard2); // print 777

        wildcardStringMethod2(wildcard); // print “A”
    }

    public void wildcardStringMethod(WildCardGeneric<String> c){
        String value = c.getWildCard();
        System.out.println(value);
    }

    public void wildcardStringMethod2(WildCardGeneric<?> c){
        Object value = c.getWildCard();
        System.out.println(value);
    }
}
```
일단 callWildCardMethod()를 보면<br>
WildCardGeneric 클래스의 타입 파라미터에 String 타입을 넣어 wildcard 인스턴스를 생성했습니다.<br>
( WildCardGeneric<String> wildcard = new WildCardGeneric<>(); )<br>
그리고 wildcardStringMethod()메소드를 호출하여 해당 객체를 파라메터로 넣고 wildCard값을 출력하고 있습니다.<br>
( public void wildcardStringMethod(WildCardGeneric<String> c) )<br>
<br>
그런데 wildcard2의 경우, 타입 파라미터에 Integer타입을 넣어 인스턴스를 생성했기 때문에<br>
wildcardStringMethod()의 파라미터에 사용할 수 없습니다.<br>
<br>
그러면 wildcard2의 wildCard값을 출력하기 위해서 WildCardGeneric<Integer> 타입의 매개변수를 받는 메소드를 또 만들어야 할까요?<br>
( public void wildcardStringMethod(WildCardGeneric<Integer> c) )<br>
<br>
이럴 때 사용하는게 wildcardStringMethod2() 메소드에서 쓴 것과 같은 wildcard 타입입니다. <br>
여기서는 매개변수에 WildCardGeneric 클래스의 타입으로 ❮?❯ 라고 썻습니다. <br>
이 ?에는 어떤 타입도 들어갈 수 있습니다.<br>
( <?>는 <? extends Object>라고 생각할 수 있습니다.<br>
즉, ‘Object 클래스를 상속받는 모든 클래스를 타입으로 사용할 수 있다’ 의 의미입니다. )<br>
<br>
와일드카드는 파라미터로만 사용할 수 있습니다.<br>
예를 들어 WildCardGeneric❮?❯ wildcard = new WildCardGeneric❮String❯(); <br>
혹은 WildCardGeneric❮?❯ wildcard = new WildCardGeneric❮❯(); <br>
과 같이 사용하면 예외가 발생합니다.<br>
(인스턴스가 만들어질 때 type parameter가 확정되어 있어야하기 때문입니다.)<br>
<br>


### 6.Not Allowed Generic Type
허용하지 않는 사용법<br>
<br>
- static 필드는 제너릭 타입을 가질 수 없습니다.<br>
// private static T t;<br>
<br>
-  Generic type은 인스턴스로 생성할 수 없습니다.<br>
```
class NotAllowedGenericType<T> {
 	new T(); //X
}
```
- 타입 파라미터로 배열을 생성하려면 new T[n] 형태로 배열을 생성할 수 없고,<br>
(T[])(new Object[n]) 으로 생성해야합니다.
```
class NotAllowedGenericType<T> {
 	private T[] array = (T[]) (new Object[원하는크기]);
}
 ```
- primitives 타입으로 제너릭 타입을 선언할 수 없습니다.<br>
// List<int> list = new ArrayList<>();<br>




### 제네릭 참고

1. Number > Integer이지만 List<Number>와 List<Integer>는 관계없음<br>
-타입파라미터의 상속관계와 List 상속관계는 상관없음<br>
-저도 무슨말인지 모르겠지만 실제로 구현해봤을 때 불가능한 것을 알게 되었습니다.(아래 면접에서 받았던 질문 항목 참고)<br>
<br>
2. 인스턴스 생성할 때 타입 인자를 2번 주지않아도 타입을 추론함(다이아몬드 연산자)<br>
- List<Integer> list = new ArrayList<Integer>(); : java6까지<br>
- List<Integer> list = new ArrayList<>(); : java7부터 가능<br>
<br>
3. Collections.emptyList()를 호출시 <br>
Collections class의 static 메서드 emptyList()를 사용할 때,<br>
인자로 넘기는 것 없는데 <T> 타입추론은 어떻게하는가?<br>
- List<T> list <- Collections.emptyList() 리턴되는 리스트가 사용되면서 타입추론<br>
- 명시적으로 메서드 앞에 <타입> 타입 파라미터를 넘겨줄 수도 있음<br>
<br>
4. 재네릭 타입도 부모 클래스가 될 수 있다. 즉, 상속이 가능하다.<br>
- class MyList<T> implements List<T>{…} 상속 가능합니다.<br>
- class MyList<T, S, U, V> implements List<T, M>{…}  확장 가능합니다.<br>
- class MyList<M> implements List<T>{…} 상위 타입 무시하고 확장 불가능합니다.<br>
<br>
5.제네릭 타입의 이름을 짓는 강제성 없는 규칙이 있습니다.(보는 사람들의 이해를 돕기위해)<br>
E - Element(요소)<br>
K - Key(키)<br>
N - Number(숫자)<br>
T - Type(타입)<br>
V - Value(값)<br>
S, U, V - 두번째, 세번째, 네번째에 선언된 타입<br>
<br>
제너릭 타입이 인스턴스화 될 때, 컴파일러는 타입파라미터와 관련된 정보를 제거한다.<br>
제너릭을 사용하기 이전의 라이브러리 등과의 호환성을 유지하기 위함.<br>
<br>


### 궁금한 점

같은 결과를 내는 Generic Method와 Wildcard의 차이는 무엇인가?<br>
<br>
```
//Generic Method
public <T extends Number> void Method(Collection<T> parameter) {...}
//Wildcard
public void Method(Collection<? extends Number> parameter) { … } 
```
보통 Generic Method는 특정 타입 파라미터 T를 두 곳 이상, <br>
예를 들어 메서드 파라미터와 리턴값에 모두 사용할 때 사용하면 좋습니다.<br>
<br>
하지만 리턴값이 없을 때 와일드 카드를 사용하면 메서드 시그니처가 훨신 간단해 집니다.
```
//Generic Method
//public static <?> void compact(List<?> list) 

//Wildcard
public static void compact(List<?> list)
```
반환값이 없기 때문에 타입 추론의 장점도 없고, 와일드카드 쪽이 의도가 훨씬 명확합니다.<br>
결국 타입 파라미터를 선언하고 2회 이상 쓸 일이 없을 경우, 시그니처 단순화의 관점에서 타입 파라미터를 선언하지 않는 와일드카드가 유용하다고 볼 수 있지 않을까 합니다.<br>
참고: https://www.slipp.net/questions/202<br>




### 면접에서 받았던 Generic 관련 질문
(기억을 더듬어 재구성 하였습니다.)<br>
<br>
1.<br>
아래와 같은 소스가 유효할까?<br>
```
List<String> stringList = new ArrayList<>();
List<Object> objectList = stringList;
```
답은 “유효하지 않다.” 입니다.<br>
<br>
위와 같은 코드는 다음과 같이 타입 애러가 발생합니다.<br>
//Incompatible types.<br>
//Required: ArrayList<java.lang.Object><br>
//Found:      ArrayList<java.lang.String><br>
<br>
면접에서 질문을 들었을 때는 Object는 모든 Java 클레스들의 부모 클레스 이므로 <br>
가능한 문법이 아닐까라고 생각했지만, <br>
애러 문구를 통해서 stringList의 타입은 그냥 String 타입이 아니라 ArrayList<java.lang.String>이고 <br>
objectList의 타입은 그냥 Object 타입이 아니라 ArrayList<java.lang.Object>라서<br>
유효하지 않은 문법입을 확인했습니다.<br>
<br>
<br>
<br>
2.<br>
Object > Animal > Dog 순으로 상속한다고 할때,<br>
Class<? super Animal>  일 떄, Cass <Dog> class = new Class<>(); 이 유효한가?<br>
<br>
“답은 유효하지 않다.” 입니다. <br>
Class<? super Animal>는 Animal과 Object Class의 인스턴스를 파라메터로 받습니다.<br>
<br>
<br>
Class <? extends Object> class 일 떄, Class <Animal> class = new Class<>(); 이 유효한가?<br>
<br>
“답은 유효하다.” 입니다. <br>
Class <? extends Object>는 Object와 그의 하위 클래스 즉,  Object, Animal, Dog 인스턴스를 모두 파라메터로 받습니다.<br>
