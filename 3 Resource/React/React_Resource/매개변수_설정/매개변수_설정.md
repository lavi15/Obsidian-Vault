
```typescript
//Test4.tsx
import React from 'react';  
import Cat from "./Cat";  
import Lion from "./Lion";  
  
const Test4 = () => {  
    return (  
        <div>  
            <Cat name='고양이'/>  
            <Lion name='사자'/>  
        </div>  
    );};  
  
export default Test4;
```

```typescript
//Cat.tsx
import React from 'react';  
  
type props = {  
    name : string  
}  
  
const Cat = (props : props) => {  
    return (  
        <div>  
            <h1>나는 {props.name} 컴포넌트이다.</h1>  
        </div>  
    );};  
  
export default Cat;
```


```typescript
//Lion.tsx
import React from 'react';  
  
type animal = {  
    name : string  
}  
  
const Lion = (props : animal) => {  
    const { name } =props  
  
    return (  
        <div>  
            <h1>나는 {name} 컴포넌트이다.</h1>  
        </div>  
    );};  
  
export default Lion;
```