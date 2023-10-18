
```Kotlin
// 조건식은 앞서배운 비교 연산자를 사용합니다
// 즉, 조건식 자리에는 true 또는 false의 결과가 들어갑니다
// 조건식이 true일때 중괄호 안의 코드를 실행합니다
if(조건식) {
	 // 실행할 코드
}


// 둘 중 한개의 코드만 실행됩니다
if(조건식) {
	// 조건식이 true일때 실행할 코드
} else {
  // 조건식이 false일때 실행할 코드
}
```

```Kotlin
var eventName = "참새"
var myName = "참새"

if(myName == eventName) {
	println("환영합니다 ${myName}님! 이벤트에 당첨됐어요!")
} else {
	println("환영합니다 ${myName}님!")
}
```

```Kotlin
var koreanScore = readLine()!!.toInt() // 국어점수 입력
var englishScore = readLine()!!.toInt() // 영어점수 입력
var mathScore = readLine()!!.toInt() // 수학점수 입력
var average = (koreanScore + englishScore + mathScore) / 3

if(average >= 90) {
	println("당신의 등급은 A입니다")
} else if(average >= 80) {
	println("당신의 등급은 B입니다")
} else if(average >= 70) {
	println("당신의 등급은 C입니다")
} else {
	println("당신의 등급은 F입니다")
}
```
