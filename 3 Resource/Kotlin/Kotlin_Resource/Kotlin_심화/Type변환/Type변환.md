
### 자료형 변환
- 숫자 자료형끼리는 **to자료형**() 메소드를 활용
- 문자열을 숫자로 변경할때에는 별도의 메소드가 필요

```Kotlin
var num1 = 20
var num2 = 30.2

var num3 = num2.toInt()
var num4 = num1.toDouble()

var strNum5 = "10"
var strNum6 = "10.21"

var num5 = Integer.parseInt(strNum5)
var num6 = strNum6.toDouble()

println("num3: $num3")
println("num4: $num4")
println("num5: $num5")
println("num6: $num6")
```

### Casting
- up casting : 자식클래스를 부모클래스의 자료형으로 객체 생성
```Kotlin
fun main() {
    println("몇 마리를 생성하시겠습니까?")
    var count = readLine()!!.toInt()
    var birds = mutableListOf<Bird>()

    for(idx in 0..count-1) {
        println("조류의 이름을 입력해주세요")
        var name = readLine()!!

				// as Bird는 생략가능
        birds.add(Sparrow(name) as Bird)
    }
    println("============조류 생성완료============")
    for(bird in birds) {
        bird.fly()
    }
}

open class Bird(name: String) {
    var name: String

    init {
        this.name = name
    }

    fun fly() {
        println("${name}이름의 조류가 날아요~")
    }
}

class Sparrow(name: String): Bird(name) {

}
```

- down casting : 부모클래스를 자식클래스의 자료형으로 객체 생성
```Kotlin
fun main() {
    println("몇 마리를 생성하시겠습니까?")
    var count = readLine()!!.toInt()
    var birds = mutableListOf<Bird>()

    for(idx in 0..count-1) {
        println("조류의 이름을 입력해주세요")
        var name = readLine()!!

        birds.add(Sparrow(name) as Bird)
    }
    println("============조류 생성완료============")
    for(bird in birds) {
        bird.fly()
    }
    // 다운캐스팅 오류
    // Sparrow는 Bird가 가져야할 정보를 모두 가지고 있지 않기 때문임
//    var s1:Sparrow = birds.get(0)
}

open class Bird(name: String) {
    var name: String

    init {
        this.name = name
    }

    fun fly() {
        println("${name}이름의 조류가 날아요~")
    }
}

class Sparrow(name: String): Bird(name) {

}
```