
```typescript
import React, {useEffect, useReducer} from 'react';  
import axios from "axios";  
  
  
type Data = {  
    userId? : number  
    id? : number  
    title?: string  
    body?: string  
}  
  
const initialState : State= {  
    data: {},  
    error: null,  
    loading: true  
}  
  
const reducer = (state : State, action : Dispatch) => {  
    switch (action.type){  
        case 'SUCCESS':  
            return {  
                data: action.payload,  
                error: null,  
                loading:false  
            }  
        case 'ERROR':  
            return {  
                data: {},  
                error: 'ERROR',  
                loading: true  
            }  
        default:  
            return state  
    }  
}  
  
type Dispatch = {  
    type : 'SUCCESS' | 'ERROR'  
    payload? : any  
}  
  
  
type State = {  
    data: Data | null  
    error: string | null  
    loading : boolean  
}  
  
const Test04 = () => {  
    const [state, dispatch] = useReducer(reducer, initialState)  
  
    useEffect(() => {  
        const url = 'https://jsonplaceholder.typicode.com/posts/3'  
  
        axios.get(url)  
            .then(res => { dispatch({type: 'SUCCESS', payload: res.data})})  
            .catch(res => { dispatch({type:'ERROR'})})  
  
    }, []);  
    return (  
        <div>  
            <h2>  
                {                    state.data && state.loading ? '로딩 중' : state.data.title  
                }  
            </h2>  
            <p>  
                {                    state.error ? state.error : null  
                }  
            </p>  
        </div>  
    );};  
  
export default Test04;
```
