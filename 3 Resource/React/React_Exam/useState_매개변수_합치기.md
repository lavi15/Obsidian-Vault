
```typescript
import React, {useState} from 'react';  
  
const Test06 = () => {  
    const [data, setData] = useState({  
        name:'',  
        id:'',  
        pwd:''  
    })  
  
    const {name, id, pwd} = data  
    const onReset = () => {  
        setData({name:'', id:'', pwd:''})  
    }    const onInputData = (e : React.ChangeEvent<HTMLInputElement>) => {  
        const { name, value } = e.target  
        setData({...data, [name]:value})  
    }  
    return (  
        <div>  
            <table border={1} cellPadding="5">  
                <tr>  
                    <th>이름</th>  
                    <td><input type="text" name="name" value={data.name} onChange={ onInputData }/></td>  
                </tr>  
                <tr>  
                    <th>아이디</th>  
                    <td><input type="text" name="id" value={data.id} onChange={ onInputData }/></td>  
                </tr>  
                <tr>  
                    <th>패스워드</th>  
                    <td><input type="password" name="pwd" value={data.pwd} onChange={ onInputData }/></td>  
                </tr>  
                <tr>  
                    <td colSpan={2}><input type="button" value="초기화" onClick={ onReset } /></td>  
                </tr>  
            </table>  
            <div>  
                <h3>이름 : { name }</h3>  
                <h3>아이디 : { id }</h3>  
                <h3>패스워드 : { pwd }</h3>  
            </div>  
        </div>  
    );};  
  
export default Test06;
```