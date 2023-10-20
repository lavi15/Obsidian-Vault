- js로 css 선언 하기
```javascript
const Test02 = () => {  
    let title = '신상명세서'  
    const name = '홍길동'  
    const age = 25  
    const addr = '서울'  
  
    const css1 = {  
        color:'red',  
        backgroundColor: 'yellow',  
        fontSize: '30pt',  
        padding: 20,  
        margin: 10,  
        border: '3px solid #000'  
    }  
    const css2 = {  
        width:400,  
        color: '#fff',  
        backgroundColor: 'hotpink',  
        fontSize: 20,  
        padding: 15,  
        margin: 30  
    }  
  
    return (  
        <>            <h2>JSX영역</h2>  
            <h2 style={css1}>{title}</h2>  
            <ul>  
                <li style={css2}>이름 : {name}</li>  
                <li style={ {backgroundColor:"greenyellow", padding:15, margin: 10} }>나이 : {age}</li>  
            </ul>  
        </>    );};
```

- css 파일로 선언 후 import
```javascript
import React from 'react';  
import './Test3.css';  
const Test3 = () => {  
    return (  
        <>            
	        <h1>클래스 속성 적용</h1>  
            <div className={'con-box'}>  
                <article>test</article>  
                <article>test</article>  
                <article>test</article>  
            </div>  
        </>    
);};
```
