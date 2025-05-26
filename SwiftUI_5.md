# ✅ SwiftUI 5단계: MVVM 아키텍처 적용

## 🎯 학습 목표

* SwiftUI에서 MVVM(Model–View–ViewModel) 패턴 이해
* 뷰(View)와 비즈니스 로직(ViewModel) 분리
* ViewModel에서 데이터 관리 및 상태 전달
* 네트워크 호출 또는 비즈니스 로직 처리 흐름 설계
* 작은 기능에서부터 구조적인 앱까지 확장하는 기반 마련

---

## 🧠 MVVM 패턴 구성

| 구성요소        | 역할                             |
| ----------- | ------------------------------ |
| `Model`     | 앱의 데이터 구조 (구조체, API 응답 모델 등)   |
| `View`      | 사용자에게 보여지는 UI (`SwiftUI View`) |
| `ViewModel` | 데이터와 UI 사이의 중간 다리. 비즈니스 로직 담당  |

---

## 📊 예시 구조

```
📁 TodoApp
├── 📁 Model
│   └── Todo.swift
├── 📁 ViewModel
│   └── TodoViewModel.swift
├── 📁 View
│   └── TodoListView.swift
```

---

## 🧪 예제: 간단한 Todo 앱 (MVVM 적용)

### ✅ 1. Model

```swift
struct Todo: Identifiable, Codable {
    let id: UUID = UUID()
    var title: String
    var isDone: Bool
}
```

---

### ✅ 2. ViewModel

```swift
import Foundation
import Combine

class TodoViewModel: ObservableObject {
    @Published var todos: [Todo] = []

    func addTodo(title: String) {
        let newTodo = Todo(title: title, isDone: false)
        todos.append(newTodo)
    }

    func toggleDone(todo: Todo) {
        if let index = todos.firstIndex(where: { $0.id == todo.id }) {
            todos[index].isDone.toggle()
        }
    }

    func removeTodo(at offsets: IndexSet) {
        todos.remove(atOffsets: offsets)
    }
}
```

> 📌 `@Published`를 통해 View에 자동 상태 전달
> 🧠 로직(추가, 삭제, 토글 등)은 View가 아닌 ViewModel에 존재

---

### ✅ 3. View

```swift
import SwiftUI

struct TodoListView: View {
    @StateObject private var viewModel = TodoViewModel()
    @State private var newTodoTitle = ""

    var body: some View {
        NavigationStack {
            VStack {
                HStack {
                    TextField("New Todo", text: $newTodoTitle)
                        .textFieldStyle(RoundedBorderTextFieldStyle())

                    Button("Add") {
                        guard !newTodoTitle.isEmpty else { return }
                        viewModel.addTodo(title: newTodoTitle)
                        newTodoTitle = ""
                    }
                    .buttonStyle(.borderedProminent)
                }
                .padding()

                List {
                    ForEach(viewModel.todos) { todo in
                        HStack {
                            Image(systemName: todo.isDone ? "checkmark.circle.fill" : "circle")
                                .onTapGesture {
                                    viewModel.toggleDone(todo: todo)
                                }
                            Text(todo.title)
                                .strikethrough(todo.isDone)
                        }
                    }
                    .onDelete(perform: viewModel.removeTodo)
                }
            }
            .navigationTitle("My Todos")
        }
    }
}
```

---

## 🧠 View와 ViewModel 연결 방식 요약

| 속성                | 설명                               |
| ----------------- | -------------------------------- |
| `@StateObject`    | View에서 ViewModel을 생성하고 소유        |
| `@ObservedObject` | 다른 뷰에서 생성된 ViewModel을 참조만 할 때 사용 |
| `@Published`      | ViewModel의 속성 변경을 View에 자동으로 반영  |

---

## ✅ MVVM 구조의 장점

* ✅ 뷰는 UI 렌더링에 집중 → 코드가 간결해짐
* ✅ ViewModel은 로직에 집중 → 테스트 가능성 높아짐
* ✅ 코드 재사용 및 유지보수가 쉬워짐
* ✅ API 호출, 데이터 변환, 로딩 상태 등도 ViewModel에서 처리 가능

---

## 🛠 확장 실습 아이디어

* [ ] **로딩 상태 추가**: ViewModel에 `@Published var isLoading = false`
* [ ] **에러 처리 로직 추가**: `errorMessage`를 별도로 `@Published`
* [ ] **네트워크 연동 ViewModel**: API 호출 후 todo 목록 로딩
* [ ] **Persistent 저장**: UserDefaults 또는 FileManager 사용

---

## 🧪 ViewModel의 네트워크 API 사용 예시 (심화)

```swift
class NewsViewModel: ObservableObject {
    @Published var articles: [Article] = []
    @Published var isLoading = false

    func fetchNews() async {
        isLoading = true
        defer { isLoading = false }

        guard let url = URL(string: "https://api.example.com/news") else { return }

        do {
            let (data, _) = try await URLSession.shared.data(from: url)
            let result = try JSONDecoder().decode([Article].self, from: data)
            DispatchQueue.main.async {
                self.articles = result
            }
        } catch {
            print("Failed to fetch: \(error)")
        }
    }
}
```

> ⚙️ `async/await`를 ViewModel에 적용하면 코드의 가독성이 높아지고 에러 처리도 명확해집니다.

---

## ✅ 5단계 요약 체크리스트

* [x] MVVM 구성요소 구분: Model, View, ViewModel
* [x] `@StateObject`와 `@ObservedObject` 차이 이해
* [x] ViewModel에 비즈니스 로직 분리
* [x] View는 UI에만 집중
* [x] 네트워크 통신 또는 로딩 상태도 ViewModel에서 처리

---

## 📚 추천 자료

* [Apple 공식 문서 – Managing Model Data](https://developer.apple.com/documentation/swiftui/managing-model-data-in-your-app)
* [RayWenderlich – SwiftUI MVVM Tutorial](https://www.raywenderlich.com/4161005-mvvm-with-combine-tutorial-for-ios)
* [Hacking with Swift – MVVM in SwiftUI](https://www.hackingwithswift.com/articles/215/understanding-mvvm-in-swiftui)

---

## ⏭️ 다음 단계

[SwiftUI 6단계: Combine 또는 async/await 연동](SwiftUI_6.md)
