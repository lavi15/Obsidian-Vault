
### Push
- 배열 뒷부분에 값을 삽입 (배열의 값이 변경된다) 
```Javascript
let arr1 = [10, 20, 30]
arr1.push(40)
arr1.push(50)
arr1.push(60)

console.log(arr1) // [10, 20, 30, 40, 50, 60]
```

### Concat
- 다수의 배열을 합치고 병합된 배열의 사본을 반환 기존의 배열은 건드리지 않
```Javascript
let arr1 = [10, 20, 30]
const arr2 = arr1.concat()
console.log(arr1) // [10, 20, 30]
console.log(arr2) // [10, 20, 30]

const arr4 = arr2.concat(40, 50, 60)
console.log(arr2) // [10, 20, 30]
console.log(arr4) // [10, 20, 30, 40, 50, 60]
```