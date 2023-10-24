
### interface
- java와 동일하게 `abstract` 통해 선언
```tsx
abstract class User {
    constructor(
        private firstName : String,
        private lastName : String,
        public nickName : String
    ) {}
	  getFullName() {
			return `${this.firstName } ${this.lastName }`
		}
}

class Player extends User {
}

const nico = new User("nico", "las", "니꼬");     // X 인터페이스는 상속만 가

const nico = new Player("nico", "las", "니꼬");   // O

nico.getFullName();    //nico las  Player는 User를 상속받았기에 사용 가
```

### HashMap
```tsx
type Words = {
    [key:string] : string   // key는 다른 아무 문자나 대체 가능 보통 key를 씀
}

class Dict {
    private words : Words
    constructor() {
        this.words = {}
    }
    add(word:Word) {
        if(this.words[word.term] === undefined) {
            this.words[word.term] = word.def
        }
    }
    def(term:string) {
        return this.words[term]
    }
}

class  Word {
    constructor(
        public term:string,
        public def:string
    ) {}
}
```