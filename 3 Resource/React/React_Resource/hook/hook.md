
[https://ko.reactjs.org/docs/hooks-state.html](https://ko.reactjs.org/docs/hooks-state.html)

- 함수형 컴포넌트는 렌더링할때마다 내부의 것들을 기억하지 못하기 때문에 다시 생성, 초기화 필요
-  변수, 함수 등 내부의 것들을 유지하기 위해서 hook 사용(useXXX)

## useState
- 값이 유동으로 변할 때
```typescript
const [상태데이터, 상태데이터의 값을 변경해주는 함수] = React.useState(초기값)

const [name, setName] = useState('홍길동')
```

```typescript
const Test03 = () => {  
    const [name, setName] = useState('홍길동')  
    const [age, setAge] = useState(25)  
    const [color, setColor] = useState('cyan')  
  
    const onName = () => {  
        setName('코난')  
    }    const onAge = () => {  
        setAge(30)  
    }    const onColor = () => {  
        setColor('red')  
    }  
    return (  
        <div>  
            <h2 style={{background : color}}>  
                {`${name}/${age}`}  
            </h2>  
            <p>  
                <button onClick={ onName }>코난</button>  
                <button onClick={ onAge }>30</button>  
                <button onClick={ onColor }>red</button>  
            </p>  
        </div>  
    );
};
```


### useRef
- DOM 요소에 접근해야 할 때 사용
- useRef 훅이 반환하는 ref 객체를 이용해서 자식 요소에 접근이 가능
- input태그와 같이 사용자가 값을 직접 입력하는 html태그에 접근 및 값 추출 가능
- Ref안에 있는 값은 **컴포넌트가 변화해도 값이 변경되지 않음**
```typescript
const Test01 = () => {  
    const [id, setId] = useState('');  
    const [pwd, setPwd] = useState('');  
  
    const idRef = useRef<HTMLInputElement | null>(null);  
  
    const onInputId = (e: React.ChangeEvent<HTMLInputElement>) => {  
        const { value } = e.target;  
        setId(value);  
    }  
    const onInputPwd = (e: React.ChangeEvent<HTMLInputElement>) => {  
        const { value } = e.target;  
        setPwd(value);  
    }  
    const onReset = () => {  
        setId('');  
        setPwd('');  
        idRef.current?.focus();  
    }  
    return (  
        <div>  
            아이디 : <input type="text" value={id} onChange={onInputId} ref={idRef} />  
            <br/><br/>  
            비밀번호 : <input type="password" value={pwd} onChange={onInputPwd}  />  
            <br/><br/>  
            <button>로그인</button> &emsp;  
            <button onClick={onReset}>초기화</button>  
        </div>  
    );
};
```


## useEffect
- 렌더링, 혹은 변수의 값 혹은 오브젝트가 달라지게 되면, 그것을 인지하고 업데이트를 해주는 함수
- 콜백 함수를 부르며, 렌더링 혹은 값, 오브젝트의 변경에 따라 어떠한 함수들을 동작시킬 수 있음
- 렌더링 후 useEffect는 무조건 한번은 실행됨
- `[ ]`로 설정하면 컴포넌트가 처음 나타날 때만 useEffect에 등록한 함수가 호출
```typescript
//컴포넌트가 나타날 때 딱 1번만 함수가 호출 
useEffect( () => { }, [ ]);

//특정 props가 바뀔 때마다 함수가 호출 
useEffect( () => { }, [ props ]);
```

#### useEffect를 사용하여 할 수 있는 3가지 동작
- 컴포넌트가 마운트 됐을 때 (처음 나타났을 때)
- 언 마운트 됐을 때 (사라질 때)
- 업데이트될 때 (특정 props가 바뀔 때)

#### cleanup 함수
- useEffect 에서 함수를 반환하는 함수
- `[ ]` 안에 내용이 비어 있는 경우에는 컴포넌트가 사라질 때 cleanup 함수가 호출

## useMemo
- 리랜더링, 최적화
- useMemo는 컴포넌트의 성능을 최적화시킬 수 있는 hooks중 하나
- useMemo에서 Memo는 Memoization를 뜻함
 **✲memoization : 기존에 수행한 연산의 결괏값을 어딘가에 저장해 두고 동일한 입력이 들어오면 재활용하는 프로그래밍 기법**
```typescript
import React, {useMemo, useState} from 'react';  
  
const Test03 = () => {  
    const [count1, setCount1] = useState(1)  
    const onAdd = () => {  
        setCount1(count1+1)  
    }  
    //count1이 바뀔때만 useMemo가 실행됨
    const isEven = useMemo(() => { return count1%2 === 0 },[count1])  
  
    return (  
        <div>  
            <h2>카운트 : {count1}</h2>  
            <button onClick={onAdd}>증가</button>  
            <hr/>  
            <h2>  
                결과 : {isEven ? '짝수' : '홀수'}  
            </h2>  
        </div>  
    );};  
  
export default Test03;
```
