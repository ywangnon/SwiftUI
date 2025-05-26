# ✅ SwiftUI 1단계: 기본 이해

## 🎯 학습 목표

* SwiftUI의 철학과 선언형 UI 구조 이해
* 주요 기본 View 구성요소와 Modifier 사용
* 상태 관리의 기초 (`@State`, `@Binding`) 이해

---

## 🧠 핵심 개념 요약

### 💬 SwiftUI란?

SwiftUI는 애플의 **선언형 UI 프레임워크**로, UI를 코드로 구성하고 상태 변화에 따라 자동으로 화면이 갱신됩니다.

```swift
// 기존 UIKit
label.text = "Hello"

// SwiftUI
Text("Hello")
```

SwiftUI는 상태를 선언하면 뷰가 그 상태에 따라 **자동으로** 업데이트되도록 설계되어 있습니다.

---

### 🧱 주요 View 컴포넌트

| 뷰        | 설명        |
| -------- | --------- |
| `Text`   | 텍스트 표시    |
| `Image`  | 이미지 표시    |
| `Button` | 버튼 생성     |
| `VStack` | 수직 스택     |
| `HStack` | 수평 스택     |
| `ZStack` | 뷰를 겹쳐서 표시 |

---

## 📦 실습 예제 1: 기본 컴포넌트

```swift
import SwiftUI

struct ContentView: View {
    var body: some View {
        VStack(spacing: 16) {
            Text("Hello, SwiftUI!")
                .font(.largeTitle)
                .foregroundColor(.blue)

            Image(systemName: "star.fill")
                .font(.system(size: 50))
                .foregroundColor(.yellow)

            Button("Press Me") {
                print("Button tapped")
            }
            .padding()
            .background(Color.blue)
            .foregroundColor(.white)
            .cornerRadius(8)
        }
    }
}
```

---

## 🧪 실습 예제 2: 상태 관리 (`@State`)

```swift
import SwiftUI

struct CounterView: View {
    @State private var count = 0

    var body: some View {
        VStack(spacing: 16) {
            Text("Count: \(count)")
                .font(.title)

            Button("Increase") {
                count += 1
            }
            .padding()
            .background(Color.green)
            .foregroundColor(.white)
            .cornerRadius(8)
        }
    }
}
```

> 🔎 `@State`는 뷰의 내부 상태를 저장하고 값이 변경되면 자동으로 UI가 갱신됩니다.

---

## 🧪 실습 예제 3: `@Binding` 이해

```swift
struct ToggleView: View {
    @State private var isOn = true

    var body: some View {
        VStack {
            Toggle("Enable Feature", isOn: $isOn)
                .padding()

            StatusView(isEnabled: $isOn)
        }
    }
}

struct StatusView: View {
    @Binding var isEnabled: Bool

    var body: some View {
        Text(isEnabled ? "Enabled" : "Disabled")
            .foregroundColor(isEnabled ? .green : .red)
    }
}
```

> 🔁 `@Binding`은 부모 뷰의 상태를 자식 뷰와 공유할 때 사용합니다.

---

## 🧰 주요 Modifier 예시

| Modifier             | 설명      |
| -------------------- | ------- |
| `.padding()`         | 여백 추가   |
| `.foregroundColor()` | 글자색 설정  |
| `.background()`      | 배경색 설정  |
| `.cornerRadius()`    | 모서리 둥글게 |
| `.font(.title)`      | 폰트 설정   |

---

## ✅ 요약 체크리스트

* [x] SwiftUI의 선언형 구조 이해
* [x] `Text`, `Image`, `Button`, `VStack` 사용
* [x] `@State`로 상태 변경
* [x] `@Binding`으로 상태 전달
* [x] 주요 Modifier 사용해 UI 커스터마이징

---

## 📚 추천 학습자료

* [SwiftUI 공식 튜토리얼](https://developer.apple.com/tutorials/swiftui)
* [Paul Hudson - 100 Days of SwiftUI](https://www.hackingwithswift.com/100/swiftui)

---

## ⏭️ 다음 단계

[SwiftUI 2단계: 상태와 데이터 흐름](SwiftUI_2.md)
