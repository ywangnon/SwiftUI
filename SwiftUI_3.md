# ✅ SwiftUI 3단계: 복합 UI 구성 (List, Form, Navigation 등)

## 🎯 학습 목표

* 실전 UI에서 자주 쓰이는 컨테이너 View 학습
* `List`, `Form`, `NavigationStack`, `TabView`, `ScrollView` 등의 사용법 이해
* 사용자와 상호작용하는 복합 구조 만들기
* `Identifiable`, `ForEach`, 뷰 간 이동 등의 활용법 익히기

---

## 🧱 핵심 컴포넌트 요약

| 컴포넌트              | 설명                                  |
| ----------------- | ----------------------------------- |
| `List`            | 행 기반의 스크롤 가능한 목록                    |
| `Form`            | 설정/입력 폼 구성용                         |
| `NavigationStack` | 화면 간 이동 구조                          |
| `TabView`         | 하단 탭바를 통한 뷰 전환                      |
| `ScrollView`      | 수직/수평 스크롤 지원                        |
| `ForEach`         | 반복 가능한 리스트 구성                       |
| `Identifiable`    | `ForEach`나 `List`와 함께 사용되는 고유 식별 모델 |

---

## 🧪 실습 예제 1: 리스트(List)와 삭제 기능

```swift
struct Fruit: Identifiable {
    let id = UUID()
    let name: String
}

struct FruitListView: View {
    @State private var fruits = [
        Fruit(name: "🍎 Apple"),
        Fruit(name: "🍌 Banana"),
        Fruit(name: "🍇 Grape")
    ]

    var body: some View {
        NavigationStack {
            List {
                ForEach(fruits) { fruit in
                    Text(fruit.name)
                }
                .onDelete { indexSet in
                    fruits.remove(atOffsets: indexSet)
                }
            }
            .navigationTitle("Fruits")
            .toolbar {
                EditButton()
            }
        }
    }
}
```

> 📌 `List`는 자동 스크롤과 삭제 기능 내장
> 🧠 `Identifiable` 프로토콜이 `ForEach`를 가능하게 함

---

## 🧪 실습 예제 2: Form과 Toggle, Picker

```swift
struct SettingsView: View {
    @State private var notificationsEnabled = true
    @State private var selectedColor = "Blue"
    let colors = ["Red", "Green", "Blue"]

    var body: some View {
        Form {
            Section(header: Text("General")) {
                Toggle("Enable Notifications", isOn: $notificationsEnabled)

                Picker("Favorite Color", selection: $selectedColor) {
                    ForEach(colors, id: \.self) {
                        Text($0)
                    }
                }
            }
        }
        .navigationTitle("Settings")
    }
}
```

> ☑️ `Form`은 설정/입력 UI에 최적화된 구성
> 🎛️ `Toggle`, `Picker`, `Stepper` 등 다양한 입력 지원

---

## 🧪 실습 예제 3: NavigationStack과 화면 이동

```swift
struct Item: Identifiable {
    let id = UUID()
    let name: String
}

struct ItemListView: View {
    let items = [Item(name: "MacBook"), Item(name: "iPhone"), Item(name: "iPad")]

    var body: some View {
        NavigationStack {
            List(items) { item in
                NavigationLink(destination: ItemDetailView(item: item)) {
                    Text(item.name)
                }
            }
            .navigationTitle("Apple Devices")
        }
    }
}

struct ItemDetailView: View {
    let item: Item

    var body: some View {
        Text("Detail view for \(item.name)")
            .font(.title)
    }
}
```

> 📂 `NavigationStack`은 iOS 16 이상에서 권장되는 화면 전환 방식
> 🔗 `NavigationLink`는 뷰 간 이동을 자연스럽게 처리

---

## 🧪 실습 예제 4: TabView로 탭 기반 앱 구성

```swift
struct TabMainView: View {
    var body: some View {
        TabView {
            Text("Home Screen")
                .tabItem {
                    Image(systemName: "house")
                    Text("Home")
                }

            Text("Settings Screen")
                .tabItem {
                    Image(systemName: "gear")
                    Text("Settings")
                }
        }
    }
}
```

> 🧭 `TabView`는 기본적으로 iOS 하단 탭 바 구조를 형성
> 🧱 각 탭은 개별 화면을 표현할 수 있음

---

## 🧪 실습 예제 5: ScrollView로 커스텀 스크롤 구성

```swift
struct CardScrollView: View {
    var body: some View {
        ScrollView(.horizontal, showsIndicators: false) {
            HStack(spacing: 20) {
                ForEach(1..<6) { i in
                    RoundedRectangle(cornerRadius: 20)
                        .fill(Color.blue)
                        .frame(width: 200, height: 120)
                        .overlay(Text("Card \(i)").foregroundColor(.white))
                        .shadow(radius: 5)
                }
            }
            .padding()
        }
    }
}
```

> 💡 수평 스크롤도 매우 쉽게 구현 가능
> 💎 ScrollView는 다양한 커스텀 UI에 유용

---

## ✅ 3단계 요약 체크리스트

* [x] `List`, `ForEach`, `Identifiable`로 반복 뷰 구성
* [x] `Form`으로 사용자 입력 UI 만들기
* [x] `NavigationStack`, `NavigationLink`로 화면 전환 구현
* [x] `TabView`로 탭 기반 앱 구성
* [x] `ScrollView`로 커스텀 레이아웃 스크롤 구현

---

## 🛠 추천 연습 과제

* [ ] **할 일 목록 앱**: `List + Form + NavigationLink` 조합
* [ ] **갤러리 뷰어**: `ScrollView`로 수평 이미지 스크롤
* [ ] **간단한 설정 화면**: `Form + Toggle + Picker`

---

## 📚 추천 자료

* [Apple Documentation – List](https://developer.apple.com/documentation/swiftui/list)
* [Hacking with Swift – Navigation](https://www.hackingwithswift.com/quick-start/swiftui/how-to-navigate-between-views-using-navigationlink)

---

## ⏭️ 다음 단계

[SwiftUI 4단계: ](SwiftUI_4.md)
