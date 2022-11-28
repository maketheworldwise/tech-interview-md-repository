- [Java Generic](#java-generic)
- [Java Collections](#java-collections)
- [Java Reflection](#java-reflection)
- [Java Wrapper Class](#java-wrapper-class)
- [Call by Reference vs Call by Value](#call-by-reference-vs-call-by-value)
- [`String` vs `StringBuilder` vs `StringBuffer`](#string-vs-stringbuilder-vs-stringbuffer)
- [`==` vs `equals()`](#-vs-equals)
- [`static` vs `Non-static`](#static-vs-non-static)
- [`final` 키워드](#final-키워드)

---

### Java Generic

> 💁🏻 : Java Generic은 모든 종류의 타입을 다룰 수 있도록 도움을 준다.

Generic은 다양한 타입의 객체들을 다루는 메서드나 Collection 클래스에서 사용하는 것으로, 컴파일 과정에서 타입체크를 해주는 기능이다. 객체의 타입을 컴파일 시에 체크하기 때문에 객체의 타입 안정성을 높이고 형변환의 번거로움을 줄여준다. 자연스럽게 코드도 더 간결해지기도 한다.

예를 들어 특정 자료구조를 만들어 배포한다고 가정했을 때, `String` 혹은 `Integer` 타입 모두를 지원하고자할 때 각 타입별로 클래스를 생성하는 것은 비효율적이게 된다. 즉, Generic은 내부에서 지정하는 것이 아닌 외부에서 사용자에 의해 지정되는 것을 의미한다.

요약하자면 Generic을 사용했을 때의 장점은 다음과 같다.

1. 잘못된 타입이 들어올 수 있는 것을 컴파일 단계에서 방지할 수 있다.
2. 클래스 외부에서 타입을 지정해주기 때문에 타입 체크를 하고 변환해줄 필요 없어 관리가 편하다.
3. 비슷한 기능을 지원하는 경우 코드의 재사용성을 높여준다.

### Java Collections

> 💁🏻 : Java Collection에는 `Map, Set, List`가 있다. 주 목적은 직접 자료구조를 구현할 필요가 없기 때문이다.

![](/docs/images/collections.png)

Java Collection에는 `Set, List, Map` 인터페이스를 기준으로 여러 구현체가 있다. 추가적으로는 `Stack, Queue` 인터페이스도 존재한다. 이러한 Collection을 사용하는 이유는 다수의 데이터를 다루는데 표준화된 클래스들을 제공해주기 때문에 자료구조를 직접 구현하지 않고도 편하게 사용할 수 있기 때문이다. 또한 배열과 다르게 객체를 보관하기 위한 공간을 미리 정하지 않아도 되므로 상황에 따라 객체의 수를 동적으로 정할 수 있다. 따라서 공간적인 효율성을 높여주는 효과가 있다.

**Set**

`Set`은 집합적인 개념에서 많이 사용하는 인터페이스다. 빠른 검색 속도를 자랑하며 순서와 데이터의 중복을 허용하지 않으며 대표적으로는 `HashSet`이 있다. 순서를 보장하고 싶을 때는 `LinkedHashSet`을 사용하면 된다.

**List**

`List`는 순서를 보장하는 인터페이스다. 데이터 중복을 허용하며 대표적으로 `ArrayList`가 있다. `ArrayList`는 기존의 `Vector`를 개선한 구현체이다.

**Map**

`Map`은 검색에 많이 활용되는 인터페이스다. 대표적으로는 `HashMap` 구현체가 있다. Key-Value 형태를 지니고 있으며, 중복된 값을 저장하지 않으며 순서를 보장하지 않는다는 특징을 가지고 있다. 순서를 보장하기 위해서는 `LinkedHashMap`을 사용한다.

(`HashMap`과 `HashTable`과의 차이는 자료구조에서 별도로 정리해놓았다.)

### Java Reflection

> 💁🏻 : Java Reflection은 클래스의 정보를 얻기 위해 사용한다.

Java Reflection은 객체를 통해 클래스의 정보를 분석해내는 것을 의미한다. 컴파일러를 무시하고 런타임 상황에서 메모리에 올라간 클래스, 메서드 등의 정의를 동적으로 찾아 조작할 수 있는 행위다.

Java Reflection을 사용하는 이유는 기본적으로 클래스와 메서드 등의 정보를 얻기 위함도 있지만 런타임에 다른 클래스를 동적으로 로딩하여 접근할 필요가 있거나 조금 더 유연한 코드를 만들기 위해 사용한다.

단, 외부에 공개되지 않도록 사용한 접근제어자 `private`를 사용했음에도 `Field.setAccessible()` 메서드를 통해 true로 지정하면 접근과 조작이 가능하기에 주의해야한다. 또한 동적으로 해석되는 유형이 포함되어있기에 JVM 최적화를 수행할 수 없다. 따라서 Reflection을 사용한 작업이 사용하지 않은 작업에 비해 성능이 떨어지기 때문에 애플리케이션에서 자주 호출되는 코드에서는 사용하면 안된다.

### Java Wrapper Class

> 💁🏻 : Java Wrapper Class는 Primitive 자료형을 객체 타입의 자료형으로 변환할 때 사용한다.

기본 자료형에 대한 클래스 표현을 Wrapper Class라 한다. 사용하는 이유는 위에서 살펴본 Generic과 Collection을 사용하려면 기본형을 사용할 수 없기 때문에 이를 Wrapping한 형태로 변환해야하기 때문이다. 또한 `null`값을 반환해야하는 경우에는 반환 타입을 Wrapper Class로 지정하여 `null`을 반환할 수 있도록 구성할 수 있다.

단, Wrapper Class를 사용하게 된다면 기본형처럼 `==`로 바로 비교할 수 없고 `intValue()` 메서드를 통해서만 해당 Wrapper Class의 값을 가져와 비교할 수 있다.

### Call by Reference vs Call by Value

> 💁🏻 : 참조에 의한 호출과 값에 의한 호출을 의미한다. 참조값을 넘기느냐 값을 넘기느냐의 차이다.

Java는 참조 자료형을 많이 사용하다보니 Call by Reference로 생각할 수 있지만 사실은 Call by Value로 동작한다. Call by Value로 동작하기 때문에 메서드를 호출하는 호출자의 변수와 호출 당하는 수신자의 파라미터는 복사된 서로 다른 변수다.

기본적으로 Java는 변수를 선언했을 때 Stack 영역에 할당된다. 원시 타입은 Stack 영역에 변수와 함께 저장되고, 참조 타입은 Heap 영역에 저장되고 Stack 영역에는 해당 객체의 주소가 저장된다.

![](/docs/images/call-by-value-primitive.png)

메서드 호출 시 넘겨받는 파라미터들도 원시 타입이라면 Stack 영역에 할당된다. 따라서 넘겨받은 파라미터 변수를 메서드 내부에서 값을 변경해도 기존 변수에는 영향이 없다.

![](/docs/images/call-by-value-param.png)

그렇다면 참조 타입의 경우에는 왜 값이 변경될까?

변수는 Stack 영역에 할당되지만 실제 객체는 Heap 영역에 위치하게 된다. 그리고 Stack에 있는 변수가 Heap 영역의 객체를 바라보고있는 형태다. 다음과 같은 코드로 과정을 이해해보면 쉽다.

```java
class User {
	public int age;

	public User(int age) {
		this.age = age;
	}

	@Override
	public String toString() {
		return "User{" +
			"age=" + age +
			'}';
	}
}

public class Main {
	public static void main(String[] args) {
		User a = new User(10);
		User b = new User(20);

		System.out.println("===================== main modify before");
		System.out.println(a); // User{age=10}
		System.out.println(b); // User{age=20}

		modify(a, b);

		System.out.println("===================== main modify after");
		System.out.println(a); // User{age=11}
		System.out.println(b); // User{age=20}
	}

	public static void modify(User a, User b) {
		a.age++;
		b = new User(30);
		b.age++;

		System.out.println("===================== modify");
		System.out.println(a); // User{age=11}
		System.out.println(b); // User{age=31}
	}
}
```

코드의 과정을 이미지로 표현하면 다음과 같다.

![](/docs/images/call-by-value-reference1.png)
![](/docs/images/call-by-value-reference2.png)
![](/docs/images/call-by-value-reference3.png)
![](/docs/images/call-by-value-reference4.png)

### `String` vs `StringBuilder` vs `StringBuffer`

> 💁🏻 : `String`은 참조 자료형이다. 선언 방식은 `new` 키워드를 이용하는 방법과 리터럴(`""`)을 사용하는 방법이 있다. 하지만 이 두가지 방식에는 차이가 있다. `new`키워드를 이용한 방법은 Heap 영역에 `String` 객체를 생성하고 리터럴은 `String Constant Pool`에 생성된다.
>
> `String`이 특별한 이유는 참조 자료형임에도 불변하다는 특징을 가지고 있기 때문이다. (공식 문서에서도 `final`로 정의되어있다.) 따라서 두 개의 `String` 변수를 더하기 연산을 처리하면 기존의 객체가 변하는게 아닌 새로운 객체가 생성이 된다. 이러한 비효율적인 방식을 해결하기 위해 `String Constant Pool`이 등장한 것이다.
>
> `StringBuilder`과 `StringBuffer`는 기존의 비효율적인 공간차지를 해결할 수 있는 자료구조이며 이 두 개의 차이는 동기화 여부다.

(전체적인 내용은 올바른 답변이기에 부가적인 내용만 정리했다.)

실제로 `new` 키워드를 이용하여 생성한 것과 리터럴을 이용하여 생성한 문자열을 `==`로 비교하면 `false`가 나온다. 반대로 `equals()` 메서드를 이용하면 문자열의 값을 비교하므로 `true`가 나온다. (`==`는 주소값을 비교하기 때문)

리터럴로 문자열을 생성한 경우에는 내부적으로 `String`의 `intern()` 메서드가 호출된다. `intern()` 메서드는 주어진 문자열이 `String Constant Pool`에 존재하는지 검색하여 있다면 그 주소값을 반환하고 없을 경우 새로 Pool에 추가하여 새로운 주소값을 반환한다.

Java 6에서는 `String Constant Pool`은 Perm 영역에 위치했지만 Java 7에서는 Heap 영역으로 변경되었다. Perm 영역은 고정된 사이즈이며 런타임 사이즈에 확장되지 않기 때문에 `Out Of Memory` 에러가 발생하기 때문이다. 또한 Heap 영역에 옮김으로서 `String Constant Pool`의 모든 문자열도 GC의 대상이 될 수 있어 이전에 비해 더 많은 이점을 챙길 수 있다.

`String Constant Pool`은 `-xx:StringTableSize` 옵션을 이용하여 사이즈를 지정할 수 있다.

### `==` vs `equals()`

> 💁🏻 : `==`는 주소값을, `equals()` 메서드는 값을 비교해준다.

이 두 개의 차이점은 주소값을 비교하느냐 혹은 값일 비교하느냐의 차이가 있으며, `==` 비교를 동일성 비교라하고 `equals()`는 동등성 비교라한다.

### `static` vs `Non-static`

> 💁🏻 : `static`한 것들은 모두 공간을 공유한다. 즉, 클래스당 하나의 공간만이 생성이 된다는 의미다...

`static` 멤버는 처음 JVM이 실행되어 클래스가 메모리에 올라가며 프로그램 종료때까지 유지된다. 클래스가 여러 번 생성되어도 `static` 멤버는 처음 한 번만 생성된다. 동일한 클래스의 모든 객체들에 의해 공유된다.

그에 반해 `Non-static` 멤버는 객체 생성할 때마다 매번 새로운 변수가 생성된다. 동일한 클래스의 모든 객체들이 서로 공유하지 않는다.

추가로 `static`으로 선언되어있는 모든 변수(전역 변수)들은 해당 클래스가 스태틱 영역의 공간에 배치될 때 그 안의 클래스의 멤버로 공간을 만들어 저장된다. 따라서 프로젝트 규모가 커질 수록 여러 메소드에서 전역 변수의 값을 변경하기 시작하면 추적하지 않는 이상 값을 파악하기 쉽지 않아 유지보수가 어려워지고 동시성 이슈가 발생할 수 있기에 가급적이면 전역 변수를 선언하지 않는 것을 권장한다.

### `final` 키워드

> 💁🏻 : 변수나 메서드 또는 클래스를 불변하게 만들기 위해 사용되는 키워드다.

`final` 키워드가 어디에 붙느냐에 따라 동작이 사뭇 다르다.

- 원시 변수에 적용 : 해당 변수의 값을 변경할 수 없음
- 참조 변수에 적용 : 해당 변수가 Heap 내의 다른 객체를 바라볼 수 없음
- 메서드에 적용 : 해당 메서드를 오버라이딩할 수 없음
- 클래스 적용 : 해당 클래스를 상속받을 수 없음
