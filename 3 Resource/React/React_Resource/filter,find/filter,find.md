
### filter
- 콜백함수를 인자로 받으며 콜백함수의 true인 요소만 모아 배열로 반환
```javascript
const newArr = arr.filter( 현재값 => 조건 )

let arr1 = [10, 20, 30, 40, 50]
const result = arr1.filter( item => item>30)
console.log(result)  // [40, 50]
  
const result2 = arr1.filter( item => item === 40)
console.log(result2) // [40]

const result3 = arr1.filter( item => item !== 40)
console.log(result3) // [10, 20, 30, 50]
```

### find
- 콜백함수를 인자로 받으며 배열을 순차적으로 확인하여 처음 true인 값을 반환
```javascript
const newArr = arr.find( 현재값 => 조건 )

let arr1 = [10, 20, 30, 40, 50]
const result = arr1.find( item => item>30)
console.log(result)  // 40
```