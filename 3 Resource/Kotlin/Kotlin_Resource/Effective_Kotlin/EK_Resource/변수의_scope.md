
상태를 정의할 때 변수와 프로퍼티의 스코프 최소화를 지향 

프로퍼티보다는 지역 변수 사용
최대한 좁은 스코프를 갖게 변수를 사용

**Bad case**
```Kotlin
var user : User
for (i in users.indices) {
	user = users[i]
	print("User at $i is $user")
}
```

**Good case**
```Kotlin
for (i in users.indices) {
	user = users[i]
	print("User at $i is $user")
}
```

**Best case**
```Kotlin
for ((i, user) in users.withIndex()) {
	print("User at $i is $user")
}
```


### 캡처링
- bad case 에서 결과값이 잘못 나온 이유는 prime이라는 변수를 캡쳐 했기 떄문
- 시퀀스를 사용하여 필터링이 지연되었고 그로인해 최종적인 prime 값으로만 필터링됨
- 2일때 필터링 된 4를 제외한 모든게 나와 있음을 확인

**Best case**
```Kotlin
val primes: Sequence<Int> = sequence {
	var numbers = genetateSequence(2) {it + 1}
	while (true) {
		val prime = numbers.first()
		yield(prime)
		numbers = numbers.drop(1).filter { it % prime != 0}
	}
	print(primes.take(10).toList())
	// [2, 3, 5, 7, 11, 13, 17, 19, 23, 29]
}
```

**Bad case**
```Kotlin
val primes: Sequence<Int> = sequence {
	var numbers = genetateSequence(2) {it + 1}
	val prime : Int
	
	while (true) {
		prime = numbers.first()
		yield(prime)
		numbers = numbers.drop(1).filter { it % prime != 0}
	}
	print(primes.take(10).toList())
	// [2, 3, 5, 6, 7, 8, 9, 10, 11, 12]
}
```
