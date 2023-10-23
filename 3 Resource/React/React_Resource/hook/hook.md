
[https://ko.reactjs.org/docs/hooks-state.html](https://ko.reactjs.org/docs/hooks-state.html)

- 함수형 컴포넌트는 렌더링할때마다 내부의 것들을 기억하지 못하기 때문에 다시 생성, 초기화 필요
-  변수, 함수 등 내부의 것들을 유지하기 위해서 hook 사용(useXXX)
- useState
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
