
- 전개연산자(...)
- 배열은 추가하지만, 객체는 덮어씀
```javascript
const arr1 = ['강아지', '고양이', '토끼', '펭귄']
const arr2 = [...arr1]
const arr3 = [...arr1, '코끼리', '기린']
const arr4 = ['사자', ...arr3.filter(item => item.indexOf('끼') !== -1 ), '호랑이']

console.log(arr1) // ['강아지', '고양이', '토끼', '펭귄']
console.log(arr2) // ['강아지', '고양이', '토끼', '펭귄']
console.log(arr3) // ['강아지', '고양이', '토끼', '펭귄', '코끼리', '기린']
console.log(arr4) // ['사자', '토끼', '코끼리', '호랑이']

const dog1 = {
	name:'멍멍이',
	age: '2'
}

const dog2 = {
	...dog1,
	name:'진도개',
	age: '5'
}

console.log(dog1) // name: '멍멍이', age: '2'
console.log(dog2) // name: '진도개', age: '5'
```
