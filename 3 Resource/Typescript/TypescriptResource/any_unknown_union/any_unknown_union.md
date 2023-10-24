
### any
- super type(모든 type이 들어갈 수 있음) (바닐라와 동일한 상태)
- 가급적 사용하지 않기를 권고

### unknown
- any와 비슷하게 동작하지만 조금 더 안전하게 동작함
- 모든 타입을 받을 수 있지만, 다른 타입의 변수에 할당하려면 타입에 대한 정의 필요 
```typescript
let unknownValue: unknown = '나는 문자열이지롱!';
console.log(unknownValue); // 나는 문자열이지롱!

let stringValue: string;
stringValue = unknownValue; // 에러 발생! unknownValue가 string임이 보장이 안되기 때문!
stringValue = unknownValue as string;
console.log(stringValue); // 나는 문자열이지롱!
```

### union
- 여러 타입 중 하나를 가질 수 있을 때 사용
- `|` 를 상용하여 여러 타입을 결합하여 사용
```typescript
type StringOrNumber = string | number; // 원한다면 | boolean 이런식으로 타입 추가가 가능해요!

function processValue(value: StringOrNumber) {
  if (typeof value === 'string') {
    // value는 여기서 string 타입으로 간주됩니다.
    console.log('String value:', value);
  } else if (typeof value === 'number') {
    // value는 여기서 number 타입으로 간주되구요!
    console.log('Number value:', value);
  }
}

processValue('Hello');
processValue(42);
```
