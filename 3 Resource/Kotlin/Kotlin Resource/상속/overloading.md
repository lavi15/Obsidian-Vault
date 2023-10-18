
**overloading 생성 규칙**
- 매개변수의 **갯수**를 다르게하면 동일한 이름의 메소드 생성 가능 
- 매개변수의 **자료형**을 다르게하면 동일한 이름의 메소드 생성 가능 
- 반환자료형(반환형)은 오버로딩에 영향을 주지 않음

**overloading 사용 사례**
- 두 개의 정수를 매개변수로 받아 더하는 메소드를 add라는 이름으로 생성
- 두 개의 실수(소수)를 매개변수로 받아 더하는 메소드도 생성 필요
- addInt, addDouble로 메소드를 따로 생성시 관리에 용의하지 않음
- 자료형이 정수,실수로 다르기 때문에 overloading으로 해결 가능

```Kotlin
fun main() {
    var calc = Calculator()
    
    var intResult = calc.add(1,2)
    var doubleResult = calc.add(1.2, 2.2)
    
    println("정수 덧셈결과: ${intResult}")
    println("실수 덧셈결과: ${doubleResult}")
    
}

class Calculator {
    
    fun add(num1: Int, num2: Int): Int {
        return num1+num2
    }
    
    fun add(num1: Double, num2: Double): Double {
        return num1+num2
    }
}
```
