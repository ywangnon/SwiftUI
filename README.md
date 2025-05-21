## ✅ 1단계: **SwiftUI 기본 이해**

### 📌 목표

* SwiftUI의 철학과 구조 이해
* 뷰 선언 방식, 미리보기 기능, 상태 관리 기초 익히기

### 🧩 학습 항목

* `@State`, `@Binding`의 의미와 사용법
* `View`, `Text`, `Image`, `Button`, `VStack`, `HStack`, `ZStack`
* `Modifier` 개념 (`.padding()`, `.foregroundColor()`, `.background()` 등)
* SwiftUI의 프리뷰(Preview) 기능

### 🔨 실습 예제

* 간단한 카운터 앱 (버튼을 누르면 숫자 증가)
* 텍스트 입력 폼 만들기 (`TextField`, `SecureField`, `Toggle`)

---

## ✅ 2단계: **상태와 데이터 흐름**

### 📌 목표

* 상태 관리 방법 심화
* 여러 뷰 간 데이터 전달 방식 이해

### 🧩 학습 항목

* `@ObservedObject`, `@EnvironmentObject`, `@StateObject`
* `ObservableObject` 프로토콜, `Published` 속성
* Navigation과 데이터 전달

### 🔨 실습 예제

* 할 일 목록 앱 (To-do List)
* 뷰 간 이동 및 데이터 추가/삭제 구현

---

## ✅ 3단계: **복합 UI 구성**

### 📌 목표

* 리스트, 폼, NavigationView 등 실제 앱 구성 요소 이해

### 🧩 학습 항목

* `List`, `Form`, `NavigationStack` / `NavigationView`
* `ForEach`, `Identifiable`, `onDelete`, `onMove`
* TabView, ScrollView 등 다양한 뷰

### 🔨 실습 예제

* 탭 기반 앱 만들기
* ScrollView 안에 카드 형태의 뷰 구현하기

---

## ✅ 4단계: **애니메이션 & 제스처**

### 📌 목표

* SwiftUI의 강력한 애니메이션 기능 이해
* 사용자 상호작용 제어하기

### 🧩 학습 항목

* `.animation()`, `.withAnimation()`
* `Gesture` (`DragGesture`, `TapGesture` 등)
* `matchedGeometryEffect`, `transition`, `rotationEffect`

### 🔨 실습 예제

* 드래그 카드 UI
* 페이드 인/아웃 애니메이션

---

## ✅ 5단계: **아키텍처와 앱 구조**

### 📌 목표

* MVVM 구조로 SwiftUI 앱 만들기
* 실제 앱 수준의 구조 설계

### 🧩 학습 항목

* MVVM 패턴 소개 및 적용
* ViewModel과 Model 분리
* 테스트 가능한 구조 설계

### 🔨 실습 예제

* 뉴스 앱: API 연동 후 목록 및 상세 보기
* 메모 앱: 뷰모델과 상태 바인딩

---

## ✅ 6단계: **Combine 또는 async/await 연동**

SwiftUI와 함께 사용하는 비동기 처리 방식까지 배우면 앱 완성도가 높아집니다.

* Combine: `@Published`, `sink`, `assign`, `Just`
* async/await: `Task`, `@MainActor`, `await`

---

## 🔚 추천 학습 순서 요약

1. SwiftUI 기본 컴포넌트 → 레이아웃
2. 상태 관리 (`@State`, `@Binding` → `ObservableObject`)
3. 리스트, 폼, 내비게이션 등 복합 UI 구성
4. 애니메이션, 제스처
5. MVVM 구조 적용
6. 네트워크/비동기 처리 연동 (Combine or async/await)

---

## 📚 추천 학습 자료

* [Apple 공식 문서 (SwiftUI)](https://developer.apple.com/documentation/swiftui/)
* [Hacking with Swift - 100 Days of SwiftUI](https://www.hackingwithswift.com/100/swiftui)
* SwiftUI 프로젝트 템플릿으로 Playground 시작
