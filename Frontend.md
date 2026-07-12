## 1. React의 상태 관리 방법과 원리
### 상태관리란?
- 리액트에서 상태(state)란 컴포넌트 속 데이터를 의미합니다. 이는 시간/사용자와의 상호작용에 따라 업데이트됩니다.
- 변화하는 상태를 앱의 다양한 부분이 서로 일관되게 공유하여, 사용자의 상호작용에 올바르게 반응하게 해주는 관리를 상태 관리라고 합니다.
- 지역적 상태 관리 (local statement management) : 컴포넌트 내부의 데이터의 변화를 관리
- 전역 상태 관리 (global statement management) : 컴포넌트와 외부 컴포넌트끼리의 상태 공유를 관리

컴포넌트에 상태를 추가하는 방법 : useState나 useReducer 훅 사용 가능 

## props vs state
props : 컴포넌트에 전달해주는 값으로, 매개변수와 같은 역할을 한다 
데이터를 자식에서 부모로 전달하는 방법 : 부모가 자식으로 setter 함수를 props를 통해 보내기 

props는 함수 매개변수처럼 컴포넌트에 전달되는 반면, 
state는 함수내에 선언된 변수처럼 컴포넌트 안에서 관리된다. 

state를 직접 바꿀수도 있는데, 왜 useState(setState)를 사용해야 하는가?
->state는 일반적인 변수와 다르게 값이 변하면 리렌더링이 발생해야 한다. 

그런데 react는 setState의 호출에 의한 주소 변경만 반응하여 리렌더링이 발생하기 때문에
useState를 통해 변경하지 않으면 react가 감지를 못한다. 

상태 변화가 생겼을 때 변화는 어떻게 알아채나 
state가 변경되면 state의 주소값이 변경되고 리액트가 이 변화를 알아채게 된다. 
그렇기 때문에 state가 배열의 형태로 존재할 때 배열의 원소를 .push()로 추가하더라도
state는 변화를 알아채질 못한다. 

### How useState works 
1. 리액트에서 어떻게 선언하는지
2. 리액트에서 상태관리가 필요한 이유와 웹 퍼포먼스 연계
3. 상태변경시 리액트 내부에서 나타나는 일
4. 리액트의 Hook 시스템 - 리액트의 재렌더링에도 변수값이 변화하지 않는 이유
5. Virtual DOM



### How can I manage the state without library
- state 란...
- props 란...
- state를 업데이트 하는 방법, setState
- props를 업데이트 하는 방법, propsDrilling

리액트에서 상태 관리를 하는 기본적인 방법은 컴포넌트의 state와 props를 사용하는 것입니다. 

props 는 부모 컴포넌트로부터 전달 받은 데이터를 의미합니다. 컴포넌트의 state는 setState 함수를 통해 업데이트할 수 있으며, 이 과정에서 리액트는 컴포넌트를 재렌더링하여 변경된 데이터를 사용자에게 보여줍니다.

그러나 이것은 컴포넌트 내부의 상태 관리일 뿐, 어플리케이션을 효과적으로 동작하게 하기 위해서는 여러 컴포넌트 간에 상태를 공유하고 관리할 수 있어야 합니다. 리액트는 단방향 흐름인 Flux 패턴을 바탕으로 하므로, 데이터 흐름이 단순하지만 단계별로 일일이 props를 넘겨주어야 하는 번거로움이 있습니다. 이는 규모가 큰 애플리케이션에서는 번거롭고, 오히려 데이터 관리가 복잡해지는 결과를 낳기도 합니다.

@ FLUX 패턴 : 단방향 데이터 흐름을 유지하는 아키텍처 디자인 패턴

따라서 트리 단계마다 명시적으로 props를 넘겨주지 않아도 컴포넌트 트리 전체에 데이터를 제공할 수 있게 만들어주는 상태 관리 라이브러리는, 복잡한 상태 관리가 필요한 경우 사용하는 것이 효율적인 면에서 필요하다고 생각합니다. 하지만 반드시 외부 라이브러리를 사용할 필요를 의미하진 않습니다. 앱의 규모가 크지 않다면, 기본적인 상태 관리 방법인 state, props만 사용하거나 내부 상태 관리 라이브러리인 useContext와 useReducer 등을 사용하는 것을 고려해 볼 수 도 있습니다.


### 상태 관리를 잘 하는 법 
- 애초에 불필요한 상태를 만들지 않기
- 상태 변경시 불변성을 고려하기
- 리렌더링 성능을 최적화하기: memo(), useCallback()
- 컴포넌트 생명주기와 상태의 주기를 같이 하기: useEffect()

출처 : https://jlee0505.tistory.com/113

## SPA vs MPA
SPA : 하나의 HTML 페이지에서 필요한 데이터만 받아 화면을 동적으로 변경하는 웹 애플리케이션 방식

ㄴ> 페이지 전체를 새로고침하지 않고, 앱처럼 동작하는 웹 



기존 방식(MPA) : 과거의 웹사이트는 페이지를 이동할 때마다 서버가 새로운 HTML을 내려줬다. 

SPA는 처음 한 번만 HTML, JS를 내려받고 그 이후에는 API만 호출. 

*SPA + JWT 조합 많이 사용되는 이유 : SPA는 서버가 HTML을 만들어주는 게 아니라, REACT/VUE가 API 호출하면 JSON 응답 -> 화면 렌더링하는 구조. 프론트와 백이 분리되어 있음. 매 요청마다 JWT만 보내면 되고, 서버가 세션 상태를 기억할 필요가 없다. 


## REACT vs VUE 
공통점 : 둘 다 컴포넌트 기반 프레임워크. 

차이점 : 

- React : HTML + JS 
- Vue : HTML. 
- 반응성이 제일 큰 차이. React는 상태가 변경되면 리렌더링해야 하는데, Vue는 반응형 객체 자체를 추적함. 
- React는 상태관리나 라우터 등에서 라이브러리가 엄청 많고 자유도가 높은데, Vue는 공식 권장안이 명확.  


## 2. React의 원리 - Virtual DOM, 단방향 데이터 바인딩 

- 개념

- 작동 원리


## 3. JSX란?
- JSX는 JavaScript를 확장한 문법으로, 공식문서에서 React와 함께 사용할 것을 권장하고 있는 문법입니다.
- JSX는 JavaScript의 모든 기능이 포함되어 있으며, React Element를 생성하기 위해 사용됩니다.


## 4. 엘리먼트와 컴포넌트의 차이 
엘리먼트는 자바스크립트 객체고, 리액트로 화면을 그려내는 데에 가장 기본적인 요소입니다. 엘리먼트는 한 번 생성되면 다시는 변형되지 않습니다.

반면 컴포넌트는 엘리먼트를 반환하는 함수 혹은 클래스를 의미합니다.
재사용성을 강조하여, 엘리먼트를 좀 더 자유롭게 다룰 수 있으며, 컴포넌트의 이름을 사용하여 하나의 태그처럼 사용할 수 있습니다.


## 5. 리액트에서 컴포넌트를 생성하는 방법

컴포넌트를 생성하는 방법으로는 함수형 컴포넌트와 클래스형 컴포넌트가 있습니다.

함수형 컴포넌트는 JavaScript 함수와 같은 방법으로 정의하며, 인자를 받아, React element를 반환하도록 만들 수 있습니다.

클래스형 컴포넌트는 ES6의 class를 사용하여 정의합니다. class안에서 render()함수를 정의하고, 여기에서 React element를 반환하도록 만들 수 있습니다.

주의해야할 점으로, 컴포넌트의 이름은 항상 대문자로 시작해야 합니다. React는 소문자로 시작하는 컴포넌트를 DOM 태그로 처리합니다. 이는 babel 컴파일을 진행할 때, 원시태그와 컴포넌트를 구분하기 위한 규칙으로, 지키지 않으면 에러가 발생합니다.


## 6. 함수형 컴포넌트 vs 클래스형 컴포넌트의 차이 

클래스형 컴포넌트는 LifeCycle API를 제공하나, 함수형 컴포넌트는 기본적으로는 제공되지 않습니다. (Hook을 사용하면 사용할 수 있습니다.)

반면, 함수형 컴포넌트는 메모리 자원을 클래스형 컴포넌트보다 덜 사용하며, 빌드한 결과물의 크기 역시 클래스형 컴포넌트보다 적습니다.

React 공식문서에서 클래스형 컴포넌트로 선언하는 방법을 아예 Legacy API로 분류해 놓을 정도로 함수형 컴포넌트는 강력합니다. 더 이상 클래스형 컴포넌트와 함수형 컴포넌트의 비교는 무의미해졌기 때문에 이후 나오는 클래스형 컴포넌트에 대한 질문들을 삭제하였습니다.



## 7. 리액트 라이프사이클 

모든 리액트 컴포넌트에는 라이프 사이클이 있으며, 컴포넌트는 마운트 -> 업데이트 -> 언마운트의 라이프사이클을 갖습니다.

마운트는 컴포넌트가 생성되는 시점를 의미하며, constructor -> getDerivedStateFromProps -> render -> componentDidMount의 순서로 호출됩니다.

업데이트는 컴포넌트가 업데이트되는 시점을 의미하며, getDerivedStateFromProps -> shouldComponentUpdate -> render -> getSnapshotBeforeUpdate -> componentDidUpdate 순서로 호출됩니다.

언마운트는 컴포넌트가 화면에서 사라지는 시점을 의미하며, 컴포넌트가 화면에서 사라지기 직전에 componentWillUnmount 가 호출됩니다.

## 8. React Hooks 

훅은 16.8버전 이전까지 클래스 기반의 리액트 로직을 함수 기반으로 대체하기 위해 만들어 졌습니다.

또한 바닐라 자바스크립트 함수와 동일한 모양이므로, 여러 훅을 이용해 커스텀 훅으로 조립하여 사용할 수 있습니다. 각각의 커스텀 훅은 독립적인 상태를 관리하며 다른 컴포넌트에 주입되어 캡슐화를 제공합니다.

즉, 훅은 코드 재사용성을 위해 만들어졌습니다.

리액트에서 훅은 호출되는 순서에 의존합니다.
그 이유는 state가 자바스크립트의 클로저를 이용하여 구현되었기 때문입니다.

클로저 내에서는 해당 state의 인덱스를 기록하고, 이 인덱스 값을 추적할 수 있도록 배열 내에서 상태값들을 관리합니다.

따라서 훅을 조건문 내에서 사용하면 이 순서에 문제가 생길 수 있으므로, 조건문 내에서 훅을 사용하면 안됩니다.



## 9. CSR vs SSR 
 
## CSR (Client Side Rendering)

브라우저(Client)가 JavaScript를 실행하여 화면을 렌더링하는 방식이다.

### 동작 과정

```text
브라우저 요청
      ↓
빈 HTML 반환
      ↓
JS Bundle 다운로드
      ↓
React 실행
      ↓
API 요청
      ↓
화면 렌더링
```

### 장점

- 서버 부하 감소
- CDN 캐싱 용이
- React SPA 구조와 잘 맞음
- 사용자 인터랙션이 많은 서비스에 적합

### 단점

- 초기 로딩 속도가 느림
- JS 다운로드 완료 전 화면 표시 불가
- SEO에 불리

### 대표 사례

- 관리자 페이지(Admin)
- 대시보드
- WebView 서비스
- SPA 애플리케이션

---

## SSR (Server Side Rendering)

서버가 HTML을 미리 생성하여 브라우저에 전달하는 방식이다.

### 동작 과정

```text
브라우저 요청
      ↓
서버 렌더링
      ↓
완성된 HTML 반환
      ↓
브라우저 표시
      ↓
Hydration
      ↓
React 활성화
```

### 장점

- 초기 화면 표시 속도 향상
- SEO에 유리
- 검색 엔진 크롤링 가능
- SNS 공유 메타데이터 제공 가능

### 단점

- 서버 부하 증가
- Hydration 비용 발생
- 구현 복잡도 증가

### 대표 사례

- 쇼핑몰
- 블로그
- 뉴스 사이트
- 검색 서비스

---

## Hydration

SSR로 생성된 HTML에 React 이벤트와 상태를 연결하는 과정이다.

```text
Server
  ↓
HTML 생성
  ↓
Client
  ↓
HTML 표시
  ↓
Hydration
  ↓
React 활성화
```

예를 들어 SSR이 아래 HTML을 생성한다.

```html
<button>구매하기</button>
```

Hydration 이후 React가 이벤트를 연결한다.

```jsx
<button onClick={buy}>
  구매하기
</button>
``` 

## CSR vs SSR 비교

| 항목 | CSR | SSR |
|--------|--------|--------|
| 렌더링 위치 | Browser | Server |
| 초기 로딩 | 느림 | 빠름 |
| SEO | 불리 | 유리 |
| 서버 부하 | 낮음 | 높음 |
| 구현 난이도 | 낮음 | 높음 |
| 대표 환경 | SPA, WebView | 쇼핑몰, 블로그 |
 

## WebView에서 CSR을 선택한 이유

WebView 환경에서는 SEO가 필요하지 않다.

SSR은 서버 렌더링과 Hydration 비용이 추가된다.

```text
SSR
서버 렌더링
      ↓
HTML 생성
      ↓
전송
      ↓
Hydration
```

반면 CSR은 브라우저에서 직접 렌더링한다.

```text
CSR
JS 다운로드
      ↓
React 렌더링
```

따라서 WebView 환경에서는 SSR의 장점을 거의 활용하지 못하므로 CSR을 사용하는 경우가 많다.
 
## 핵심 정리

- CSR은 브라우저가 JavaScript를 실행하여 화면을 렌더링하는 방식이다.
- SSR은 서버가 HTML을 미리 생성하여 전달하는 방식이다.
- SSR은 SEO와 초기 렌더링에 유리하다.
- CSR은 서버 부하가 적고 구조가 단순하다.
- Hydration은 SSR HTML에 React 이벤트를 연결하는 과정이다.
- WebView 환경에서는 SEO가 불필요하므로 CSR을 선택하는 경우가 많다.

 

 






## 10. MVVM 구조 




## XSS : Cross-Site Scripting (근데왜 xss임?)
- 웹 애플리케이션에 악성 javascript를 삽입항여 다른 사용자의 브라우저에서 실행되도록 만드는 공격
- 신뢰되지 않은 입력이 브라우저에서 실행 가능한 javascript 컨텍스트로 들어가는 취약점
- 사용자 입력이 코드로 해석되지 않아야 함.

### 종류

1. Stored XSS
- Payload가 DB 등에 저장됨.
- 다른 사용자가 조회할 때 실행.
- 영향 범위가 가장 큼.

2. Reflected XSS
- Request 파라미터가 응답에 그대로 반영됨.
- 악성 URL을 통해 유도.

3. DOM-based XSS
- 서버 응답은 안전하지만, 클라이언트 JS가 location.search, location.hash 등을 innerHTML 등에 넣으면서 발생.


### 방어 

- 방어의 핵심은 Context-aware Output Encoding
- HTML Context → HTML Escape
- Attribute Context → Attribute Escape
- JS Context → JS Escape
- URL Context → URL Encode
HTML을 허용해야 하는 경우에는 Escape가 아니라 Sanitize가 필요하다. (예: DOMPurify)


### Escape, Sanitize
- Escape : 인코딩- > 브라우저가 문자 그대로 해석하게 만드는 것. 코드 텍스트를 입력해도 escape 처리하면 코드로 인식하지 않는다. 
- 



### BFF - Backend for Frontend 
- 프론트엔드가 여러 api를 호출하는 대신, bff 하나만 호출하게끔 하는 구조
- MSA(마이크로서비스) 환경에서 많이 사용한다.
- 내부 API를 외부에 노출하지 않아도 된다. ⭐ -> BFF가 없으면 클라이언트가 여러 서비스의 엔드포인트를 직접 호출해야 한다. 내부 필드가 클라이언트에 노출될 가능성을 줄일 수 있다. 

## DOM - Document Object Model 
- 브라우저가 html 문서를 객체 형태로 표현한 것
- html -> 브라우저가 읽음 -> 자바스크립트가 읽을 수 있는 객체 트리 (dom)으로 변환
- document, element, node 이런 객체들을 조작하는 것이 dom manipulation
- document - html - body - h1 이런 트리를 dom tree 라 함
- dom 을 게속 수정하면 reflow(레이아웃 재계산) repaint(다시 그리기) 반복되어 성능이 떨어질 수 있음. 그래서 react 는 virtual dom 을 이용해 실제 dom 변경횟수를 줄이려고 함. 
- DOM은 브라우저가 관리하는 구조. 그래서 react는 DOM 변경횟수를 줄일 수 있도록, virtual DOM을 사용해, 변경한 부분만 적용하도록 한다.


## SPA - Single Page Application
- 페이지를 새로고침하지 않고, 하나의 HTML 페이지에서 필요한 부분만 동적으로 바꿔주는 웹 애플리케이션.
- MPA는 새 HTML 요청이 있으면 전체 화면을 다시 그려야 한다. 
- React는 SPA 구현에 적합한 라이브러리고, 그 과정에서 Virtual DOM을 활용해 화면 업데이트를 효율적으로 처리하는 것 

브라우저 동작 원리
Event Loop
Execution Context
Closure
this
React Rendering
CSR vs SSR
CORS
