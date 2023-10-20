
- 객체를 사용할때 임시로 Scope를 만들어서 편리한 코드 작성 가능

## Scope Function

### Scope Function 종류
|                      | Scope에서 접근방식 this | Scope에서 접근방식 it |
| ----------------------- | ----------------------- | --------------------- |
| 블록 수행 결과를 반환   | run, with               | let                   |
| 객체 자신을 반환        | apply                   | also                  |

- **let**
	- 중괄호 블록안에 it으로 자신의 객체를 전달하고 수행된 결과를 반환
```Kotlin
	var strNum = "10"

    var result = strNum?.let {
        // 중괄호 안에서는 it으로 활용함
        Integer.parseInt(it)
    }

    println(result!!+1)
```
- **with**
	- 중괄호 블록안에 this로 자신의 객체를 전달하고 코드를 수행
	- this는 생략해서 사용할 수 있으므로 반드시 null이 아닐때만 사용
```Kotlin
	var alphabets = "abcd"

    with(alphabets) {
//      var result = this.subSequence(0,2)
        var result = subSequence(0,2)
        println(result)
    }
```
- **also**
	- 중괄호 블록안에 it으로 자신의 객체를 전달하고 객체를 반환
	- apply와 함께 자주 사용
```Kotlin
fun main() {
    var student = Student("참새", 10)

    var result = student?.also {
        it.age = 50
    }
    result?.displayInfo()
    student.displayInfo()
}

class Student(name: String, age: Int) {
    var name: String
    var age: Int

    init {
        this.name = name
        this.age = age
    }

    fun displayInfo() {
        println("이름은 ${name} 입니다")
        println("나이는 ${age} 입니다")
    }
}
```
- **apply**
	- 중괄호 블록안에 this로 자신의 객체를 전달하고 객체를 반환
	- 주로 객체의 상태를 변화시키고 바로 저장하고 싶을때 사용
```Kotlin
fun main() {
    var student = Student("참새", 10)

    var result = student?.apply {
        student.age = 50
    }
    result?.displayInfo()
    student.displayInfo()
}

class Student(name: String, age: Int) {
    var name: String
    var age: Int
    
    init {
        this.name = name
        this.age = age
    }
    
    fun displayInfo() {
        println("이름은 ${name} 입니다")
        println("나이는 ${age} 입니다")
    }
}
```
- **run**
```Kotlin
//객체에서 호출하지 않는 경우 
fun main() {
	var totalPrice = run {
        var computer = 10000
        var mouse = 5000

        computer+mouse
    }
    println("총 가격은 ${totalPrice}입니다")
```

```Kotlin
//객체에서 호출하는 경우 
//with와 달리 null체크를 수행할 수 있으므로 더욱 안전하게 사용 가능
fun main() {
    var student = Student("참새", 10)
    student?.run {
        displayInfo()
    }
}

class Student(name: String, age: Int) {
    var name: String
    var age: Int
    
    init {
        this.name = name
        this.age = age
    }
    
    fun displayInfo() {
        println("이름은 ${name} 입니다")
        println("나이는 ${age} 입니다")
    }
}
```


### 수신객체와 람다
- T는 수신객체를 의미
- block : 내부는 람다함수의 소스코드
- 수신객체는 it으로 사용 가능
```Kotlin
// 수신객체 자체를 람다의 수신객체로 전달하는 방법
public inline fun <T, R> T.run(block: T.() -> R): R
public inline fun <T> T.apply(block: T.() -> Unit): T
public inline fun <T, R> with(receiver: T, block: T.() -> R): R

// 수신객체를 람다의 파라미터로 전달
public inline fun <T> T.also(block: (T) -> Unit): T
public inline fun <T, R> T.let(block: (T) -> R): R
```

### Scope function 안에 Scope function를 사용할 경우
- Child Function에서 Shadow가 되어서 제대로 참조하지 못함
- it을 다른 이름으로 변경해서 사용
```Kotlin
// Scope Function을 중첩으로 사용할 경우 누가 누구의 범위인지 알수 없다!
// Implicit parameter 'it' of enclosing lambda is shadowed 경고 발생!

data class Person(
	var name: String = "",
	var age: Int? = null,
	var child: Person? = null
)

// 잘못된 예시
Person().also {
	it.name = "한석봉"
	it.age = 40
  val child = Person().also {
	  it.name = "홍길동" // 누구의 it인지 모른다!
    it.age = 10 // 누구의 it인지 모른다!
  }
  it.child = child
}

// 수정한 예시
Person().also {
	it.name = "한석봉"
	it.age = 40
  val child = Person().also { c ->
	  c.name = "홍길동"
    c.age = 10
  }
  it.child = child
}
```
