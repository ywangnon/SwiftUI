# ✅ SwiftUI 2단계: 상태와 데이터 흐름

## 🎯 학습 목표

* 여러 뷰 간 상태 공유 이해
* `ObservableObject`, `@ObservedObject`, `@StateObject`, `@EnvironmentObject` 등 다양한 상태 관리 방식 익히기
* MVVM 패턴의 기초 감잡기

---

## 🧠 상태 속성 정리

| 속성                   | 사용 대상       | 설명                                     |
| -------------------- | ----------- | -------------------------------------- |
| `@State`             | **단일 뷰 내부** | 뷰 내부에서 소유하고 관리하는 상태                    |
| `@Binding`           | **자식 뷰**    | 상위 뷰에서 상태를 전달받아 공유                     |
| `@StateObject`       | **소유자 뷰**   | `ObservableObject`를 직접 생성해 소유          |
| `@ObservedObject`    | **비소유 뷰**   | 다른 곳에서 생성한 `ObservableObject`를 주입받아 사용 |
| `@EnvironmentObject` | **전역 공유**   | 앱 전체에 걸쳐 공유되는 객체를 주입받아 사용              |

---

## 🧩 핵심 프로토콜: `ObservableObject`

```swift
class CounterModel: ObservableObject {
    @Published var count: Int = 0
}
```

* `@Published`로 선언된 속성이 변경되면 해당 객체를 구독하는 뷰가 자동으로 업데이트됨
* 이 객체는 `@StateObject`, `@ObservedObject`, `@EnvironmentObject`로 뷰에 연결됨

---

## 🧪 실습 예제 1: `@StateObject` + `@ObservedObject`

```swift
import SwiftUI

class CounterModel: ObservableObject {
    @Published var count = 0
}

struct ParentView: View {
    @StateObject private var counter = CounterModel()

    var body: some View {
        VStack(spacing: 16) {
            Text("Count: \(counter.count)")

            Button("Increment") {
                counter.count += 1
            }

            ChildView(counter: counter)
        }
    }
}

struct ChildView: View {
    @ObservedObject var counter: CounterModel

    var body: some View {
        Text("Child View Count: \(counter.count)")
    }
}
```

> ☑️ `ParentView`는 `StateObject`로 상태를 **소유**하고
> `ChildView`는 `ObservedObject`로 상태를 **구독**합니다.

---

## 🧪 실습 예제 2: `@EnvironmentObject`

```swift
class UserSettings: ObservableObject {
    @Published var isDarkMode = false
}

struct SettingsView: View {
    @EnvironmentObject var settings: UserSettings

    var body: some View {
        Toggle("Dark Mode", isOn: $settings.isDarkMode)
            .padding()
    }
}

struct PreviewWrapper: View {
    var body: some View {
        SettingsView()
            .environmentObject(UserSettings())
    }
}
```

> 💡 `@EnvironmentObject`는 전역으로 등록해 여러 뷰에서 접근 가능하게 함.
> `.environmentObject(...)`로 주입이 **필수**.

---

## 🧩 상태 바인딩 흐름 요약

```text
@State -> @Binding    → 뷰 간 값 전달
         ↘︎
  ObservableObject    → 여러 뷰에서 공유

@StateObject          → 해당 뷰에서 직접 생성
@ObservedObject       → 외부에서 주입받음
@EnvironmentObject    → 앱 전체에서 공유
```

---

## ✅ 요약 체크리스트

* [x] `ObservableObject`와 `@Published` 이해
* [x] `@StateObject`와 `@ObservedObject`의 차이 이해
* [x] 자식 뷰에는 `@Binding` 또는 `@ObservedObject`로 상태 전달
* [x] `@EnvironmentObject`로 전역 공유 가능
* [x] ViewModel의 기초로 활용 가능 (MVVM 전환 시작)

---

## 🛠 추천 실습

* [ ] Todo 앱에서 `TodoModel`을 `ObservableObject`로 만들고 상태 변경 시 뷰 자동 갱신
* [ ] Settings 화면을 `EnvironmentObject`로 구현 (다크 모드 전역 관리)

---

## 📚 추천 자료

* [Apple 공식 문서 - Data Flow](https://developer.apple.com/documentation/swiftui/managing-model-data-in-your-app)
* [Swift with Majid: ObservableObject](https://swiftwithmajid.com/2019/12/18/the-power-of-observableobject-in-swiftui/)

---

## ⏭️ 다음 단계

[SwiftUI 3단계: 복합 UI 구성](SwiftUI_3.md)
