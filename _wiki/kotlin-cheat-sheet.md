---
layout  : wiki
title   : 코틀린 치트 시트
summary : 내가 보려고 만든 코틀린 문법 치트 시트
date    : 2023-07-27 16:41:41 +0900
updated : 2023-08-09 17:34:15 +0900
tag     : kotlin
toc     : true
public  : true
parent  : 
latex   : false
resource: C95E2AF4-5305-4288-B8BA-4E9303B53C4A
---
* TOC
{:toc}

# 코틀린 치트 시트
`코틀린 == 러스트 > 고랭` 정도로 language curve가 있는 것 같다.

## 인텔리제이 단축키

- ^ : Control.
- ⇧ : Shift.
- ⌥ : Option(Alt)
- ⌘ : Command.
- ⏎ : Enter 

- ^ + r : 빌드 후 실행
- ⌥ + Enter : 현재 컨텍스트(커서)에 대한 액션을 표시하여 수정 제안 목록 표시.
- F2 : 오류/경고 사이를 jump
- ⌘ + 1 : 프로젝트 창으로 포커스
- ESC : 에디터에 포커스
- ⌘ + E : 최근 실행했던 파일들 확인
- ⌘ + B : 커서의 심볼의 선언으로 이동
- ⌥ + F7 : 커서 심볼이 사용된 위치 검
- ^ + ^ : 뭐든 실
- ⌥ + Up /  ⌥ + Down : 커서의 코드 선택 영역 확대 / 축소
- ⇧ + ⌘ + ⏎ : 현재 커서의 구문 완성


## 선언과 정의
1. `val`과 `var`
    - `val`은 immutable한 'value'
    - `var`은 mutable한 'variable'이라고 생각하면 될 것 같다.

2. 함수 선언
    ```kotlin
    fun sum(a: Int, b: Int) : Int {
        return a + b
    }

    fun sum2(a: Int, b: Int) : Int = a + b

    fun sum3(a: Int, b: Int) = a + b

    fun sum4(a: Int, b: Int) : Int {
        return a + b
    }
    ```
    - 함수 선언과 리턴 표현 등은 위와 같이 할 수 있다.

    ```kotlin
    fun printSum(a: Int, b: Int)
        println("$a + $b = ${a + b}")
    }
    fun printSum(a: Int, b: Int) : Unit { // `Unit` is like `void`
        println("$a + $b = ${a + b}")
    }
    ```
    - 위 두 printSum은 동일한 표현이다. 리턴타입을 명시 안 할 시 Unit 타입으로 리턴타입이 정해진다고 한다(void와 같다고 보면 되는 것 같다)

3. Default Parameter
    ```kotlin
    fun greeting(message: String = "안녕하세요") { // default parameter 가능
        println(message)
    }
    ```
    - 코틀린에서도 디폴트 파라미터 사용이 가능하다.
    - 디폴트 파라미터를 아규먼트로 사용한다면 해당 파라미터 기입은 생략하고 다른 파라미터 값부터 기입하는 식으로 사용 할 수도 있다.

## 흐름 제어
1. 코틀린의 if/else는 '식'으로서 값을 리턴할 수 있음
    ```kotlin
    val str = if (20 > 19) {
        "성인"
    } else {
        "아이"
    }
    ```
    
    - 그래서 삼항 연산자가 없다. if를 쓰면 되니까

    ```kotlin
    val c = if (15 > 10) 15 else 10
    ```

2. 타 언어의 switch보다 강력한 `when`
    ```kotlin
    val day = 2
    val result = when (day) {
        1 -> "월요일"
        2 -> "화요일"
        3 -> "수요일"
        4 -> "목요일"
        5, 6, 7 -> "금금금"
        else -> "기타"
    }
    ```
    - else는 필수이나 아래 enum처럼 경우가 두 가지밖에 없으면 생략 가능

    ```kotlin
    enum class Color {
        RED, GREEN
    }
    . . .
    when (getColor()) {
        Color.RED -> println("red")
        Color.GREEN -> println("green")
    }
    ```

3. for 루프는 범위 연산자 `..`과 `until`,  루프내 변수 증가 `step`, 감소 `downTo`
    ```kotlin
    for (i in 0..3) { // 0 <= i <= 3
        println(i)
    }
    // `until`은 끝 범위 미포함
    for (i in 0 until 3) { // 0 <= i < 3
        println(i)
    }
    // `step`으로 이터레이션 인덱스 값 증가
    for (i in 0..6 step 2) { // 0, 2, 4, 6
        println(i)
    }
    // `downTo`로 이터레이션 인덱스 값 감소
    for (i in 3 downTo 1) {
        println(i)
    }
    ```

4. 배열내에서만 반복
    ```kotlin
    val numbers = arrayOf(10, 20, 30)
    for (i in numbers) {
        println(i)
    }
    ```

5. while은 평범함

6. Nullable에 대한 메소드 호출 `?.`와 Elvis 연산자 `?:`, 단언 연산자 `!!`
    - Nullable한 값의 메소드 호출은 `?.`로 수행해야함
        ```kotlin
        var a: String? = "Apple"
        // println(a.length) // a가 Nullable하나 null이 아닐때에도 이렇게 호출하면 에러
        println(a.length)
        ```
    
    - Elvis 연산자는 좌변이 null일 경우 우변을 리턴함
        
        ```kotlin
        val c = a?.length ?: 0
        ```
    
    - 값이 런타임에 절대 null이 아니라고 확신할 수 있는 경우에는 단언 연산자 `!!.`로 메소드를 호출할 수 있음. 값이 null이면 NullPointerException 발생
    
        ```kotlin
        var a: String? = null
        println(a!!.length) // NPE 발생 
        val b: String = "APPLE"
        println(b!!.length)
        ```
    - . 말고 `!!.`을 사용하고 nullable은 `?.`을 사용하는 것이  best practice인 것 같음


## 예외처리
1. 기본
    ```kotlin
    try {
        throw Exception()
    } catch (e: Exception) {
        println("에러 발생")
    } finally {
        println("finally 실행!")
    }
    ```
2. 코틀린에서 try, catch는 표현식이기 때문에 값 반환 가능
    ```kotlin
    val a = try {
        "1234".toInt()
    } catch (e: Exception) {
        println("예외 발생!")
    }
    ```
3. 기타
    ```kotlin
    // throw를 리턴하는 함수는 Nothing 타입을 반환하는 것
    fun failFast(message: String) : Nothing {
        throw IllegalArgumentException(message)
    }
    fun main() {
        val b: String? = "NOT NULL"
        val c: String = b ?: failFast("b is null")
        // 위에서 반환되는 Nothing 타입을 분석하여
        // 아래 코드가 실행될 수 있는지 컴파일러에서 분석함
        println("이 메시지는 출력되나?") 
    ```
    
## 클래스
1. 기본 생성자에 대해서 `constructor` 생략 가능(권장)
    ```kotlin
    class Coffee constructor(val name: String) { // 생성자를 선언할 때 `constructor` 사용
    class Coffee (val name: String) { ... } { // 생성자를 선언할 때 `constructor` 사용
    ```

2. Property(멤버 변수)
    - `class 클래스명(프로퍼티 나열)` 형태로 사용할 수 있음
        ```kotlin
        class Coffee (
            // val name: String,
            var name: String = "", // 기본값도 넣어줄 수 잇음
            var price: Int = 0,
            var iced: Boolean = false, // Trailing comma 사용 권장
        )
        ```
    - var 프로퍼티는 자동으로 Java getter와 setter가 생성됨. val 프로퍼티는 Java getter만 생성.
    - 커스텀 getter, setter를 정의할 수 있다.
        ```kotlin
        class Coffee {
            // 커스텀 getter
            val brand: String
                get() = "스타벅스"
                // get() { // 이렇게 내부에서 로직으로 반환할 수도 있음
                //     return "스타벅스"
                // }
            // 커스텀 setter
            var quantity: Int = 0
            set(value) {
                if (value > 0) {
                    // field는 키워드는 아니고 식별자. 식별자를 통해 backing field에 접근
                    field = value
                    // 이렇게 하면 quantity 부분에서 또 setter가 호출되어 무한 할당이 시도됨;
                    // quantity = value
                }
            }
        }
        fun main() {
            val coffee = Coffee() // 인스턴스 생성
            // 코틀린 컴파일러가 생성해준 getter, setter를 통해 값이 들어가게 된 것임
            // 프로퍼티가 `val`타입이었으면 getter만 있음
            coffee.name = "Iced Americano"
            coffee.price = 2000
            println("${coffee.name} 가격은 ${coffee.price} 브랜드는 ${coffee.brand}")
        ```

3. 상속
    - 코틀린의 모든 클래스의 조상은 `public open class Any`
    - Any에는 equals(): Booleanm, hashCode(): Int, toString(): String이 있음
    - 디폴트로 클래스는 상속불가(Java의 final), `open`으로 선언해야 상속 가능
    - 프로퍼티들도 `open`으로 선언해줘야 하위 클래스에서 재정의 가능
        ```kotlin
        open class Dog {
            open var age: Int = 0
            open fun bark() {
                println("멍멍")
            }
        }
        ```
    - `override` 키워드를 써야 상위 클래스에서 open한 프로퍼티를 재정의 가능
    - 하위 클래스에서 override한 프로퍼티나 메소드는 자동으로 `open` 상태가 됨. 더 이상의 상속을 막으려면 `final` 키워드로 선언
    - 하위 클래스에서 상위 클래스의 메소드나 프로퍼티 접근은 `super`
        ```kotlin
        open class Bulldog(override var age: Int = 0) : Dog() { // <- `()` 사용
            // final override fun bark() { // `final`로 선언하면 더 이상의 상속을 방지할 수 있음
            override fun bark() {
                super.bark() // `super`로 상위 클래스 메소드 실행
                println("컹컹")
            }
        }
        ```

4. 추상 클래스
    - `abstract` 추상 클래스
    - 추상 클래스의 프로퍼티, 메소드 사용도 `override` 키워드로 사용

5. 빈 클래스
    - 본문이 없는 클래스도 생성 가능
    ```kotlin
    class EmptyClass
    ```

## 인터페이스
1. 선언
    ```kotlin
    class Product(val name: String, val price: Int)
    
    interface Wheel {
       fun roll()
    }
    
    interface Cart : Wheel { // 인터페이스간 상속은 클래스 상속과 다르게 `()`가 들어가지 않음
        var coin: Int // 인터페이스 프로퍼티는 무조건 구현해야함
        
        val weight: String // 인터페이스 getter에서는 field 사용 못 함
            get() = "20KG"
        
        fun add(product: Product) // 몸통 없는 디폴트 함수도 지원
        
        fun rent() {
            if (coin > 0) {
                println("카트를 대여합니다")
            }
        }
        
        override fun roll() {
            println("카트가 굴러감")
        }
        
        fun printId() = println("1234")
    }
    
    interface Order {
        fun add(product: Product) {
            println("${product.name} 주문이 완료되었습니다")
        }
        fun printId() = println("5678")
    }
    ```
    - 인터페이스 getter에서는 `field` 사용 못 함
    - 인터페이스간 상속은 클래스 상속과 다르게 `()`가 들어가지 않음

2. 사용
    ```kotlin
    // 둘 이상의 인터페이스도 구현 가능
    // 인터페이스 사용. 클래스 상속과는 다르게 `()`가 들어가지 않음
    class MyCart(override var coin: Int) : Cart, Order {
        override fun add(product: Product) { // 인터페이스 메소드도 `override`로 상속
            if (coin <= 0) println("코인을 넣으셈")
            else println("${product.name}이 카트에 추가됨")
            super<Order>.add(product) // `super<인터페이스명>`로 상위 인터페이스 접근 가능
        }
        
        // 두 개의 인터페이스에서 동일한 메소드를 가질 경우, 하위 클래스에서는 그 메소드 구현이 강제됨
        override fun printId() {
            super<Cart>.printId()
            super<Order>.printId()
        }
    }
    
    fun main() {
        val cart = MyCart(coin = 100)
        cart.rent()
        cart.roll()
        cart.add(Product("장난감", 1000))
        cart.printId()
    }
    ```
3. 인터페이스 메소드도 `override`로 상속
    - `super<인터페이스명>`로 상위 인터페이스 접근 가능
    - 두 개의 인터페이스에서 동일한 메소드를 가질 경우, 하위 클래스에서는 그 메소드 구현이 강제됨

## enum 클래스
1. 서로 연관된 상수들의 집합. 클래스이기 때문에 생성자와 프로퍼티 정의 가능
    ```kotlin
    enum class PaymentStatus(val label: String) {
        UNPAID("미지급") {
            override fun isPayable() = true
        },
        PAID("지급완료") {
            override fun isPayable() = false
        },
        FAILED("지급실패") {
            override fun isPayable() = false
        },
        REFUNDED("환불") {
            override fun isPayable() = false
        }; // <-- 프로퍼티 선언 다음 메소드 선언을 위해 세미콜론 사용해야함
        // fun isPayable() = false
        abstract fun isPayable(): Boolean
    }
    ```
2. 사용
    ```kotlin
    fun main() {
        println(PaymentStatus.UNPAID.label) // 프로퍼티를 사용
        if (PaymentStatus.UNPAID.isPayable()) {
            println("결제 가능 상태")
        }
        
        // valueof : 인풋값으로 클래스의 인스턴스화 시켜주는 메소드
        val paymentStatus = PaymentStatus.valueOf("PAID")
        println(paymentStatus.label)
        if (paymentStatus == PaymentStatus.PAID) {
            println("결제 완료 상태")
        }
        // values : enum 클래스에서 선언된 상수들을 가져옴
        // enum 상수의 기본 프로퍼티
        // 1. `name` : enum 상수의 이름을 그대로 반환
        // 2. `ordinal` : enum 상수의 순서를 그대로 반환
        for (status in PaymentStatus.values()) {
            println("${status} ${status.name}(${status.label}) : ${status.ordinal}")
        }
    }
    ```
    - `valueof` : 인풋값으로 클래스의 인스턴스화 시켜주는 메소드
    - `values` : enum 클래스에서 선언된 상수들을 가져옴
    - enum 상수의 기본 프로퍼티들
        - `name` : enum 상수의 이름을 그대로 반환
        - `ordinal` : enum 상수의 순서를 그대로 반환

## Collections
1. Immutable과 Mutable
    ```kotlin
    // Immutable
    val currencyList = listOf("달러", "유료", "원")
    // Mutable
    // val mutableCurrencyList = mutableListOf<String>()
    // mutableCurrencyList.add("달러")
    // mutableCurrencyList.add("유로")
    // mutableCurrencyList.add("원")
    // 가독성 좋은 mutable의 add
    val mutableCurrencyList: MutableList<String> = mutableListOf<String>().apply {
        add("달러")
        add("유로")
        add("원")
        // this.add("달러") // 이거랑 똑같음
    }

    // Immutable set
    val numberSet = setOf(1, 2, 3, 4)
    // Mutable set
    val mutableSet = mutableSetOf<Int>().apply {
        add(1)
        add(2)
        add(3)
        add(4)
    }
    // Immutable map
    val numberMap = mapOf("one" to 1, "two" to 2) // to : key, value 세팅할 때 쓰는 중위표현식
    // Mutable map
    val mutableNumberMap = mutableMapOf<String, Int>()
    mutableNumberMap["one"] = 1 // literal 문법으로 값 넣는게 권장
    // mutableNumberMap.put("one", 1) // 이런 자바 스타일은 안좋음
    
    // Linked List
    val linkedList = LinkedList<Int>().apply {
        addFirst(1)
        add(2)
        addLast(3)
    }
    // Array List
    val arrayList = ArrayList<Int>().apply {
        add(1)
        add(2)
        add(3)
    }
    ```
 
2. Collection Builder
    ```kotlin
    val numberList: List<Int> = buildList {
        add(1) // add는 mutable에만 있는데 위처럼 immutalbe List에도 쓸 수 있는건
        add(2) // 빌드리스트 내부에서 MutableList를 사용하고 빌드리스트가
        add(3) // 반환하는 타입은 immutable List임. 빌더 안에서는 마음껏 add 가능
    }

    // 타입이 아래 `put`으로부터 추론됨(`MutableMap<String, Int>`)
    val map = buildMap {
        put("a", 1)
        put("b", 0)
        put("c", 4)
    }
    ```
    - 초기 값 넣는것만 편하게 하고 싶고 이후에 immutable로 쓰고 싶으면 빌더 쓰는게 편하다

3. Collection 반복
    ```kotlin
    for (currency in currencyList) {
        println(currency)
    }
    ```
    - 일반 패턴
    
    ```kotlin
    val iterator = currencyList.iterator()
    while (iterator.hasNext()) {
        println(iterator.next())
    }
    ```
    - 이터레이터 패턴

    ```kotlin
    currencyList.forEach {
        println(it) // it : lambda랑 관련있음
    }
    ```
    - forEach 패턴

    ```kotlin
    val lowerList = listOf("a", "b", "c")
    val upperList = lowerList.map { it.uppercase() }
    ```
    - for loop 대신 map 인라인 함수(중간 연산자)
    
    ```kotlin
    // 자바는 중간 연산자를 쓰고 최종 연산자가 있어야 로직대로 동작이 됨
    // 코틀린에서는 `asSequence()`라는 것처럼 자바의 stream()같은 중간 연산자가 있긴함
    val filteredList = upperList.filter { it == "A" || it == "C" }
    val filteredList = upperList
        .asSequence()  // asSequence가 없으면 filter 인라인 함수 하나당 내부적으로 collection 객체를 개별적으로 생성(메모리 낭비)
        .filter { it == "A" || it == "C" }
        .filter { it == "B" }
        .filter { it == "Z" || it == "Y" }
        .filter { it == "G" }
        .toList()
    ```
    - filter 인라인 함수

    
## Data Class
- 데이터를 전달하고 보관할 목적으로 쓰는 클래스
- 기본적으로 생성되는 메소드 `equals`, `toString`, `hashCode`, `componentN`, `copy`. 모두 Java의 Lombok이 생성해주던 것들
- JDK15에는 record라는게 코틀린 data 클래스랑 비슷한 기능

1. 선언과 사용
    ```kotlin
    data class Person(val name: String, val age: Int) // {
    //class Person(val name: String, val age: Int) {
    //    override fun equals(other: Any?): Boolean {
    //        if (this == other) return true
    //        if (javaClass != other?.javaClass) return false
    //        other as Person
    //        if (name != other.name) return false
    //        if (age != other.age) return false
    //        return true
    //    }
    //
    //    override fun hashCode(): Int {
    //        var result = name.hashCode()
    //        result = 31 * result + age
    //        return result
    //    }
    //}

    fun main() {
        val person1 = Person("tony", 12)
        println("이름=${person1.component1()}, 나이=${person1.component2()}")
        
        val (name, age) = person1 // 구조분해 할당
        println("이름=${name}, 나이=${age}")
        
        val set = hashSetOf(person1) // HashSet
        println(set.contains(person1)) // true
    }
    ```
    - `val (name, age) = person1` : 구조분해 할당

## Singleton and Companion object
1. `object` : 객체 선언

2. `object` 선언으로 **싱글턴 객체**를 만들 수 있음
    ```kotlin
    object Singleton {
        val a = 1234
        fun printA() = println(a)
    }
    ```
    - object로 된 싱글턴 객체는 주로 유틸리티 객체를 만드는데 쓰이거나 상수들의 집합을 만들 때 쓰인다.
    - ```kotlin
      object DatetimeUtils {
            val now: LocalDateTime
                get() = LocalDateTime.now()
            const val DEFAULT_FORMAT = "YYYY-MM-DD" // `const`는 상수 변수 선언에 사용
            fun same(a: LocalDateTime, b: LocalDateTime): Boolean {
                return a == b
            }
      }
      ```

3. Companion object : 동반 객체. `companion object` 키워드로 생성
    ```kotlin
    class MyClass {
        private constructor() // private으로 생성자 막음
        
        // companion object { // 이름 가질수도 있고 안 가질수도 있음
        companion object MyCompanion {
            val a = 1234
            // MyClass를 생성자를 통해 생성하는 것을 막기 위해 newInstance 사용
            fun newInstance() = MyClass()
        }
    }
    ```
    - `private constructor()` : private으로 생성자를 막음
    -  이름 가질수도 있고 안 가질수도 있음
        ```kotlin
        companion object { // 이름 가질수도 있고 안 가질수도 있음
        companion object MyCompanion { ... }
        ```

       


