
### 컴포넌트
- UI 를 구성하는 조각(piece)에 해당되며, 독립적으로 분리되어 재사용을 됨을 목적으로 사용  
- React 앱에서 컴포넌트는 개별적인 JavaScript 파일로 분리되어 관리

### 함수형 컴포넌트
- React 컴포넌트는 개념상 JavaScript 함수와 유사 
- 컴포넌트 외부로부터 속성(props)을 전달 받아 어떻게 UI를 구성해야 할지 설정하여 React 요소(JSX를 Babel이 변환 처리)로 반환
- 이러한 문법 구문을 사용하는 컴포넌트를 React는 '함수형(functional)'으로 분류

### JSX 규칙 : JavaScript + XML
- JSX는 리액트 컴포넌트를 작성하면서 return 문에 사용하는 문법
- JSX가 하는 일은 React 요소(Element)를 만드는 것
- JSX 는 JavaScript 문법 확장(JavaScript eXtension)으로 구문이 HTML과 유사
- 하지만 React 요소는 실제 DOM 요소가 아니라, **JavaScript 객체**

#### 규칙  
1. 태그는 반드시 닫아줘야 함
2. 최상단에서는 반드시 div로 감싸주어야 함( Fragment 사용 시 <>) 
3. JSX안에서 자바스크립트 값을 사용하고 싶을 때는 {}를 사용
4. 조건부 렌더링을 하고 싶으면 &&연산자나 삼항 연산자를 사용 
5. 인라인 스타일링은 항상 객체형식으로 작성
6. 스타일 작성 시 – 빼고 첫 글자는 대문자로 작성
7. 별도의 스타일파일을 만들었으면 class 대신 className을 사용( 권장사항 ) 
8. 주석은 아래와 같이 사용해 작성한다.
```react
{/* */}
```

### Fragment
- 최상단에 div 대신 <>로 생성
- div가 많을 때 사용하고 html 개발자 모드로 확인시 div에 감싸지지 않은 상태로 생성됨
```html
<div></div>
<></>
```