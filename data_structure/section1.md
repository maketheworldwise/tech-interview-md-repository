- [Hash](#hash)
- [SynchronizedMap \& ConcurrentHashMap](#synchronizedmap--concurrenthashmap)

---

### Hash

> 💁🏻 : Hash는 검색에서 많이 사용되는 단방향 암호 기법이다. 레인보우 어택과 같은 공격으로 뚫릴 수 있어 솔트와 키 스트레칭을 통해 암호화를 진행한다. Hash와 관련된 자료구조로는 HashTable이 존재하는데 Key-Value 형태로 저장하여 빠르게 데이터를 검색할 수 있다. 암호화 관점에서든 자료구조든 Hash가 문제되는 것은 바로 Hash 충돌이다. 이 충돌을 해결하기 위해서는 분리 연결법이나 개방 주소법을 이용한다.

**Hash**

Hash는 고정된 길이의 암호화된 문자열로 바꿔버리는 단방향 암호화 기법이다. Hash 알고리즘은 종류가 다양하지 MD5, SHA-1, HAS-180은 실제로 보안이 뚫렸을 정도로 확실하게 안전을 보장하지 않는다.

따라서 완벽하다고는 할 수 없지만 앞에서 언급한 레인보우 어택과 같은 공격을 대비하여 솔트라는 임의의 값과 해시 함수를 여러 번 수행하는 키 스트레칭 기법을 이용해 보안 수준을 높인다.

**Hash Function (HashCode)**

Hash Function은 특정 길이의 데이터를 고정된 길이의 데이터로 매핑하는 함수다. 매핑 전 데이터를 키로 지정하고 매핑 후의 데이터 값(HashCode)을 해시 값이라고 하며 매핑하는 과정을 Hashing이라고 한다.

이렇게 Hash Function을 통해 얻어낸 값은 인덱스 값을 생성하는데 사용한다. 대표적인 함수로는 Division Method, Digit Folding, Multiplication Method, Universal Hashing이 있다.

하단의 이미지로 따진다면 John Smith를 Hash Function을 통해 02라는 인덱스 번호를 만들어내고, 02번째 인덱스에 데이터를 저장하는 흐름이다.

![](/docs/images/hash.png)

**Hash Collision**

Hash에서 가장 문제가 되는 것이 바로 Hash Collision이다. 이 Hash 충돌은 Hash Function을 통해 얻어낸 인덱스 값이 중복되는 현상을 의미한다. 실제로 `Object` 클래스를 열어 `hashCode()` 메서드를 보면 int를 반환하는데, int 범위를 넘어설 경우를 생각해볼 수 있다.

이러한 Hash Collision 현상은 분리 연결법과 개방 주소법을 이용하여 해결할 수 있다. 분리 연결법은 하단의 이미지처럼 Bucket 데이터에 대해 자료구조를 활용해 메모리를 추가하여 주소를 저장하는 방법이다. 체이닝 방식이라고도 하며, 간단하게 구현이 가능하다는 장점을 가지고 있다. 문제는 데이터의 수가 많아지면 동일한 Bucket에 데이터가 많아지고 캐시의 효율성이 감소될 소지가 있다.

![](/docs/images/hash-collision-solution1.png)

개방 주소법은 `HashTable`의 공간을 활용하는 방법이다. 비어있는 곳을 찾아 데이터를 저장하는 방식이다. 하단의 이미지에서 본다면, John Smith 데이터가 있기에 그 다음 Hash에 저장하는 것을 볼 수 있다. 또 다른 저장공간 없이도 데이터를 저장하고 처리할 수 있어서 좋지만 Hash Function의 성능에 전체 `HashTable`의 성능이 좌지우지된다는 단점이 있다.

![](/docs/images/hash-collision-solution2.png)

**HashTable vs HashMap**

이 두 자료구조의 차이는 동기화 여부다. `HashTable`의 실제 코드를 열어보면 `synchronized` 키워드가 붙어있는 것을 알 수 있다. 또 다른 차이로는 `HashTable`이 1.1에 나왔다면 `HashMap`은 1.2에서 추가되었다. `HashMap`은 `HashTable`에 비해 매우 빠른 처리 속도를 가지기 때문에 데이터 무결성을 고려할 필요가 없는 단일 쓰레드에서 많이 사용하는 편이다.

### SynchronizedMap & ConcurrentHashMap

`HashTable`의 데이터 무결성이라는 이점도 챙기고 빠른 속도를 자랑하는 `HashMap`처럼 속도와 데이터의 무결성 두 가지를 만족시킬 수 있는 방법은 `SynchronizedMap`과 `ConcurrentHashMap`을 이용하는 방법이 있다.

`SynchronizedMap`의 사용법은 어떤 Map 인터페이스든지 `Collections.synchronizeMap()`으로 감싸주면 동기화 Map이 된다. 실제로 `SynchronizedMap`은 `HashTable`보다 더 빠른 처리 속도를 가지고 있다. `SynchronizedMap`은 Map을 `Synchronized`로 감싸고 내부에 Mutex라는 락 객체를 이용해 동기화를 제공한다. 또한 `get(), put()` 메서드를 살펴보면 `synchronize` 키워드를 이용하는 것을 확인할 수 있는데, 문제는 같은 Bucket의 데이터에 접근하는 경우에는 동기화 작업이 필요하지만 서로 다른 Bucket에 데이터를 넣을 경우에는 동기화 작업이 불필요하다. 즉, `SynchronizedMap`은 전체를 단위로 동기화를 수행하다보니 병렬로 수행할 수 있는 작업을 수행하지 못하는 비효율적인 부분이 있다.

그에 비해 1.5부터 사용가능한 `ConcurrentHashMap`은 락이 걸리는 단위가 전체가 아닌 Bucket 단위로 줄어든다. 즉, 부분 락을 제공해주기 때문에 멀티쓰레드 환경에서 안정적으로 동작하고 데이터를 병렬로 처리할 수 있어 `HashTable, SynchronizedMap`에 비해 더 좋은 성능을 가지고 있다.
