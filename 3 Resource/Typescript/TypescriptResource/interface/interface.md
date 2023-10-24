
### Typescript의 interface
- java와 동일하게 interface와 implements를 사용하여 생성 및 상속 가능
- type은 다양하게 사용할 수 있지만 interface는 형태만 선언 가능
- interface도 type으로 지정 가능
    ```tsx
    type User = {
    	name : string,
    	hobby : string
    }
    
    interface User {
    	name : string,
    	hobby : string
    }
    ```

### abstract / interface
- interface 사용의 장점 : interface는 js에서는 없는 값으로 데이터의 양이 줄어듬
- interface 사용의 단점 : interface에서는 private, protected 사용 불
    ```tsx
    abstract class User {
    	constructor (
    		protected firstName:string,
    		protected lastName:string
    	) {}
    	abstract sayHi(name:string):string
    	abstract fullName():string
    }
    class Player extends User {
    	fullName() {
    		return `${this.firstName} ${this.lastName}`
    	}
    	sayHi(name:string) {
    		return `Hello ${name}. My name is ${this.fullName()}`
    	}
    }
    ```
    
    ```tsx
    interface User {
    		firstName:string,
    		lastName:string,
    		sayHi(name:string):string,
    		fullName():string
    }
    class Player implements User {
    	constructor (
    		firstName:string,
    		lastName:string
    	) {}
    	fullName() {
    		return `${this.firstName} ${this.lastName}`
    	}
    	sayHi(name:string) {
    		return `Hello ${name}. My name is ${this.fullName()}`
    	}
    }
    ```
- 