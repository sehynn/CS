Fire and forget : (=async no reply 구조) 비동기적으로 작업을 전송한 뒤, 응답을 기다리지 않고 바로 다음 로직으로 진행하는 구조 
- 로그 수집 및 모니터링/푸시 알림 발송/이벤트 버스(메시지 큐에 publish만 하고 다른 서비스가 추적하지 않음)
- 장점 : 발신자가 응답을 기다리지 않아 i/o 블로킹이 줄고, 메시지 큐 활용하면 임시 장애 시에도 메시지 보존 가능
- 단점 : 무응답성, 데이터 손실 가능성

-> 일반 비동기 요청과 차이는?

-> 왜 응답을 기다리지 않는 구조를 사용하는가?

-> 어떤 상황에서 적합한가?

- DLQ란?


## BLoC 패턴 
 Bussiness Logic Component
 UI 와 Bussiness Logic 을 분리하여, 각각 코드의 의존성을 낮추게한다.

https://pks2974.medium.com/bloc-%EC%9D%B4%ED%95%B4-%ED%95%98%EA%B8%B0-%EB%B0%8F-%EA%B0%84%EB%8B%A8-%EC%A0%95%EB%A6%AC-%ED%95%98%EA%B8%B0-7dc705e4c640

-> Flutter에서 setState와 차이는?

- Stream 기반 구조란?

- Bloc과 MVVM 차이?

- Provider / Riverpod / Redux

- 테스트 용이성이 왜 좋아지는가?
- 비동기 API 호출은 어디서 처리하는가?

 
# WebView Bridge (Flutter ↔ React)

## WebView란?

WebView는 네이티브 앱 내부에 내장된 브라우저 컴포넌트이다.

사용자가 Chrome이나 Safari 같은 외부 브라우저를 실행하지 않고도 앱 내부에서 웹 콘텐츠를 표시할 수 있다.

Android에서는 `WebView`, iOS에서는 `WKWebView`를 사용한다.

### 장점

- Android / iOS에서 동일한 웹 코드 사용 가능
- 스토어 심사 없이 웹 배포만으로 수정 가능
- React, Next.js 등 웹 기술 활용 가능
- 운영 및 유지보수 비용 절감

### 단점

- 네이티브 UI 대비 렌더링 성능이 낮음
- 초기 로딩 속도가 느릴 수 있음
- 기기 기능 접근 시 Bridge 구현 필요
- 복잡한 애니메이션이나 고성능 작업에 불리

---

# WebView Bridge란?

WebView 내부 JavaScript와 네이티브 앱(Flutter, Android, iOS)이 서로 통신하기 위한 연결 통로이다.

기본적으로 Web(JavaScript)와 Native는 서로 다른 런타임에서 동작하기 때문에 직접 메서드를 호출할 수 없다.

```text
React(Web)
      ↕
 WebView Bridge
      ↕
Flutter Native
```

따라서 Bridge를 통해 메시지를 주고받는다.

---

## Web → Flutter

웹에서 네이티브 기능을 호출하는 방식

### React

```javascript
window.flutter_inappwebview.callHandler(
  "login",
  accessToken
);
```

### Flutter

```dart
controller.addJavaScriptHandler(
  handlerName: 'login',
  callback: (args) {
    final token = args[0];
  },
);
```

### 활용 사례

- 로그인 토큰 전달
- 카메라 실행 요청
- 푸시 토큰 요청
- 앱 종료 요청

---

## Flutter → Web

Flutter에서 웹으로 데이터를 전달하는 방식

### Flutter

```dart
controller.evaluateJavascript(
  source: """
    window.receiveToken('$token');
  """
);
```

### React

```javascript
window.receiveToken = (token) => {
  localStorage.setItem("token", token);
};
```

### 활용 사례

- JWT 전달
- 사용자 정보 전달
- 앱 상태 전달

---

# WebView 렌더링이 느린 이유

네이티브 앱은 설치 시 이미 코드가 빌드되어 있다.

반면 WebView는 실행 시마다 다음 과정을 수행한다.

1. HTML 다운로드
2. JS 다운로드
3. CSS 다운로드
4. JS 실행
5. React 렌더링

따라서 초기 로딩 시간이 발생한다.

---

# WebView 성능 개선 사례

## 1. SSR 제거 후 CSR 전환

WebView 환경에서는 SEO가 필요하지 않다.

SSR의 주요 장점인 검색 엔진 최적화와 초기 HTML 제공 효과를 활용하기 어렵기 때문에 SSR 비용만 추가로 발생할 수 있다.

### SSR 구조

```text
Server
 └ SSR 렌더링
      ↓
 HTML 전송
      ↓
 React Hydration
```

### CSR 구조

```text
JS 다운로드
      ↓
 React 렌더링
```

WebView에서는 SSR + Hydration 비용을 제거하여 구조를 단순화할 수 있다.

---

## 2. 토큰 수신 우선순위 처리

### 문제 상황

```text
WebView 진입
      ↓
API 요청
      ↓
토큰 없음
      ↓
요청 실패
      ↓
토큰 수신
      ↓
재요청
```

초기 API 요청이 실패하면서 불필요한 네트워크 요청이 발생한다.

### 개선

```text
WebView 진입
      ↓
Flutter → JWT 전달
      ↓
토큰 저장
      ↓
React App Mount
      ↓
API 요청
```

### 효과

- 불필요한 API 재요청 제거
- 초기 렌더링 시간 감소
- 사용자 체감 성능 향상

---

## 3. 정적 리소스 캐싱

```http
Cache-Control: max-age=31536000
```

적용 대상

- JS Bundle
- CSS
- 이미지

효과

- 재진입 시 다운로드 비용 감소
- 초기 로딩 시간 단축

---

## 4. Bundle Size 감소

### Before

```text
main.js 3MB
```

### After

```text
main.js 800KB
```

### 방법

- Code Splitting
- Dynamic Import
- Tree Shaking

효과

- 다운로드 시간 감소
- JavaScript 파싱 시간 감소
- 초기 렌더링 속도 향상

---

# 핵심 정리

- WebView는 앱 내부에서 웹 콘텐츠를 렌더링하는 브라우저 컴포넌트이다.
- WebView Bridge는 React(Web)와 Flutter(Native)가 통신하기 위한 인터페이스이다.
- 실제 서비스에서는 JWT 전달, 푸시 토큰 전달, 네이티브 기능 호출 등에 활용된다.
- WebView 성능 개선 방법으로는 SSR 제거, 토큰 선전달, 정적 리소스 캐싱, 번들 최적화 등이 있다.




왜 Flutter와 React(Web)를 연결했는가?
Native ↔ Web 간 통신은 어떻게 이루어지는가?
인증 토큰은 어떻게 전달했는가?
React에서 Flutter로 데이터 전달 어떻게 하나?
JavascriptChannel 설명 가능?
WebView 보안 이슈 아는가?
XSS 위험은?
JS Injection 위험 대응?
민감한 데이터 전달 시 고려사항?
외부 URL 로딩 제한 어떻게 했는가?
Bridge 통신 타이밍 이슈 겪어봤는가?
WebView 렌더링 느린 문제 어떻게 해결?
Android/iOS 차이 있었는가?
앱 뒤로가기 처리 어떻게 했는가?
쿠키/세션 동기화 문제 경험?
딥링크와 WebView 같이 쓴 경험?
