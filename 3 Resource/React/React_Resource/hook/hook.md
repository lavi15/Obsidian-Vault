
[https://ko.reactjs.org/docs/hooks-state.html](https://ko.reactjs.org/docs/hooks-state.html)

- 함수형 컴포넌트는 렌더링할때마다 내부의 것들을 기억하지 못하기 때문에 다시 생성, 초기화 필요
-  변수, 함수 등 내부의 것들을 유지하기 위해서 hook 사용(useXXX)

### useState
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
