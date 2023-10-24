
## tsconfig.json
- `tsc --init` 명령을 실행하면 생성되는 파일
- tsconfig.json은 **TypeScript 프로젝트의 설정 파일**
- 주로 프로젝트의 **컴파일 옵션** 및 **입력 파일**들을 정의하는데 사용
- https://www.typescriptlang.org/ko/tsconfig

### compilerOptions - strict
- TypeScript 컴파일러가 코드를 엄격하게 검사하도록 하는 설정
- **true**로 사용 권장
- true시 아래의 옵션이 true로 변경
	- strictNullChecks
		- null이 될 수 있는 것에 대해 엄격히 검사
	- strictFunctionTypes
	- strictBindCallApply
	- strictPropertyInitialization
	- noImplicitAny
		- 함수의 인자 또는 변수의 타입이 명시적으로 선언되지 않은 경우에 컴파일러가 자동으로 any타입을 부여하지 않음
	- noImplicitThis
	- alwaysStrict

### compilerOptions - sourceMap
- TypeScript 컴파일러에게 소스 맵 파일을 생성하는 설정
- true로 설정시 .js 파일과 .js.map 파일이 같이 생성됨
- map 파일을 통해 javascript와 typescript간 매핑을 이해해 typescript간 소스 코드 위치로 이동 가능
- 개발 환경에서는 **true**로 사용 권장

### compilerOptions - target
- TypeScript 프로젝트 내 코드들이 어떤 JavaScript 버전으로 변환을 할 지 정하는 옵션
- `es5` 로 설정하면 CommonJS 버전으로 컴파일
- `es2016(=es7)` 로 설정하면 ES2016 버전으로 컴파일
    - **최신 브라우저는 보통 ES2016을 지원하니 이렇게 설정하시는 것을 추천**

###  compilerOptions - module
- TypeScript 파일을 컴파일한 후 생성되는 JavaScript 모듈의 형식을 지정
- 모듈을 가져오고 내보내는 방식을 결정하는 옵션

### compilerOptions - outDir
- 컴파일된 JavaScript 파일이 저장될 출력 디렉터리를 지정

### include , exclude
- tsc가 컴파일을 할 때 포함하거나 제외할 파일이나 디렉터리를 지정하는 옵션
- **include**
``` 
["src/**/*"]
//  src 디렉토리 밑의 파일을 컴파일 
```
- **exclude**
```
["node_modules", "dist"]
// node_modules, dist 디렉토리 밑의 파일은 컴파일 대상에서 제외하겠다는 의미
```
