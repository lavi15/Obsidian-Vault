
var : 변경 가능 객체
val : 읽기 전용 객체

val이 읽기 전용 객체이나 immutable 객체를 뜻하지는 않음
다만 val 객체를 변경하기 보단 객체를 copy해서 변경을 하는 방법 사용이 필요

```Kotlin
val list = listof(1,2,3)

if(list is MutableList) list.add(4)
//옳지 않은 예


val list = listof(1,2,3)

val mutableList = list.toMutableList()
mutableList.add(4)
//옳은 예
```

val이라는 읽기 전용 객체는 다른 객체에서 copy해서 사용하기 좋은 객체이니 항상 copy 후 사용

immutable 객체는 **set, map의 키로 사용가능**
mutable 객체는 사용 불가
다만 immutable 객체의 요소를 변경시  해시 테이블 내에서 요소를 찾을 수 없게 됨
(최초 객체를 넣을 당시의 주소를 기반으로 하기 때문)
```Kotlin
val names: SortedSet<FullName> = TreeSet()
val person = FullName("AAA", "AAA") 
//FullName ={name:String, subName: String}
names.add(person)
names.add(FullName("BBB", "BBB"))
names.add(FullName("CCC", "CCC"))

print(s) // [AAA AAA, BBB BBB, CCC CCC]
print(person in names) //true

person.name = "ZZZ"
print(names) // [ZZZ AAA, BBB BBB, CCC CCC]
print(person in names) //false
//아직 names 안에 person이 있으나 처음과 요소가 달라져 주소가 달라졌기 떄문에 false로 뜸
```

Int는 immutable 객체이나 plus, minus 메서드를 통해 변경한 객체를 반환 가능
Iterable도 immutable 객체이나 map, filter를 통해 새로운 객체를 반환
우리가 **만드는 immutable 객체도 이와 같이 동작 해야함**
```Kotlin
class User(
	val name : String
	val surname : String
) {
	fun withSurname(surname : String) = User(name,surname)
}

val user = User("Maja", "Markiewicz")
user = user.withSurname("Moskala")
print(user) // User(name=Maja, surname=Moskala)
```

다만 이렇게 하나하나 메서드를 만드는건 귀찮기 떄문에 **data 한정자를 사용**
data 한정자는 `copy` 라는 메서드를 만들어줌

```Kotlin
data class User(
	val name : String
	val surname : String
) 

val user = User("Maja", "Markiewicz")
user = user.copy("Moskala")
print(user) // User(name=Maja, surname=Moskala)
```

이렇게 immutable data 모델 클래스를 만들어 사용하는건 많은 장점을 가지므로 기본적으로 이렇게 생성

> 정리 
> 1. var 보다는 val를 사용
> 2. mutable 프로퍼티 보다는 immutable 프로퍼티를 사용
> 3. mutable 객체와 클래스 보다는 immutable 객체롸 클래스 사용
> 4. 변경이 필요한 대상을 만들어야 한다면, immutable data class로 만들고 copy
> 5. 컬렉션에 상태를 저장해야 한다면, mutable 컬렉션 보다는 읽기 전용 컬렉션 사용
> 6. 변이 지점을 적절하게 설계하고, 불필요한 변이 지점은 지양
> 7. mutable 객체의 외부 노출은 지양

