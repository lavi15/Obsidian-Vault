
### 변수 Type 명시적 선언

```tsx
let a : boolean = true
```

### name, age라는 key중 name은 필수지만 age는 선택적일때

```tsx
const player : {
  name : string,
  age? : number    //?를 붙이면 해당 값은 있어도 되고 없어도 됨
} = {
  name : "nico"
}

//위와 동일한 형태의 코드이나 타입을 선언하여 재사용성을 높임
type Player = {
  name : string,
  age? : number 
}
const nico : Player = {
  name : "nico"
}
const lynn : Player = {
  name : "lynn",
  age : 12
}
```

### string을 인자로 받고 Player Type을 반환

```tsx
type Player = {
  name : string,
  age? : number 
}
function playerMaker(name:string) : Player {
  return {
	  name
  }
}

//위의 함수와 동
function playerMaker = (name:string) : Player => ({name})
```

### Type undefined, null

```tsx
let a : undefined = undefined;
let b : null = null;       //다음과 같이 undefined, null도 type으로 설정 가능
```

### Type unknown

```tsx
let a : unknown;  //api등으로 인자를 불러올때 변수의 타입을 모를때 사용

let b = a+1;   //해당 코드는 사용 불가, a가 unknown이기 때문

if(typeof a === 'number') {    //이렇게 a의 타입이 number일떄 실행으로 걸어줘야 실행 가능 
  let b = a+1;       
}
```

### Type never

```tsx
function hello() : never {    //절대 실행되지 않는 함수 타입
  return "x";
}

function hello() : never {    //해당 error를 발생함으로 코드 동작  
  throw new Error("xxx");
}
```

### Type 값을 선언
```tsx
type Team = "read" | "blue" | "yellow"   // read ,blue, yellow 만 사용 가능
type Health = 1 | 4 | 10                // 1, 4, 10만 사용가능
```
