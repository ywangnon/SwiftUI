# ✅ SwiftUI 4단계: 애니메이션과 제스처

## 🎯 학습 목표

* SwiftUI에서 제공하는 **선언형 애니메이션** 이해 및 적용
* 상태 변화에 따른 **자동 애니메이션 처리** 방식 익히기
* 다양한 **제스처 인식**(Tap, Drag, LongPress 등) 적용
* 고급 효과인 `matchedGeometryEffect`, `transition`, `withAnimation` 등을 활용하여 **생동감 있는 UI** 구현

---

## 🧠 핵심 개념 요약

| 기능                      | 설명                        |
| ----------------------- | ------------------------- |
| `.animation()`          | 상태 변화 시 애니메이션을 자동 적용      |
| `withAnimation {}`      | 명시적으로 애니메이션 효과를 줄 때 사용    |
| `.transition()`         | 뷰의 등장/제거 시 애니메이션          |
| `.gesture()`            | 터치/스와이프/드래그 등 사용자 상호작용 감지 |
| `DragGesture()`         | 드래그 제스처                   |
| `TapGesture()`          | 탭 제스처                     |
| `matchedGeometryEffect` | 두 뷰 간의 자연스러운 전환           |

---

## 🧪 예제 1: 기본 애니메이션 적용 (`.animation()`)

```swift
struct ScaleAnimationView: View {
    @State private var isScaled = false

    var body: some View {
        VStack {
            Circle()
                .fill(Color.pink)
                .frame(width: isScaled ? 200 : 100, height: isScaled ? 200 : 100)
                .animation(.easeInOut(duration: 0.5), value: isScaled)

            Button("Toggle Size") {
                isScaled.toggle()
            }
            .padding(.top)
        }
    }
}
```

> ✅ 상태 변화에 따라 크기를 자동으로 애니메이션 처리
> 💡 `.animation(_, value:)`는 해당 값이 변경될 때만 애니메이션을 적용

---

## 🧪 예제 2: 명시적 애니메이션 (`withAnimation`)

```swift
struct RotateView: View {
    @State private var angle: Double = 0

    var body: some View {
        VStack {
            Image(systemName: "arrow.2.circlepath.circle.fill")
                .resizable()
                .frame(width: 100, height: 100)
                .rotationEffect(.degrees(angle))
                .foregroundColor(.orange)

            Button("Rotate") {
                withAnimation(.spring()) {
                    angle += 90
                }
            }
        }
    }
}
```

> 🌀 `withAnimation` 블록 안의 모든 뷰 변화는 애니메이션 적용됨

---

## 🧪 예제 3: 뷰 전환 애니메이션 (`transition`)

```swift
struct TransitionExampleView: View {
    @State private var showBox = false

    var body: some View {
        VStack {
            if showBox {
                RoundedRectangle(cornerRadius: 25)
                    .fill(Color.green)
                    .frame(width: 200, height: 200)
                    .transition(.scale.combined(with: .opacity))
            }

            Button("Toggle Box") {
                withAnimation {
                    showBox.toggle()
                }
            }
        }
    }
}
```

> 🎯 `.transition()`은 뷰의 등장과 제거 시에만 적용됨

---

## 🧪 예제 4: 제스처 기본 - Tap, LongPress

```swift
struct GestureView: View {
    @State private var tapped = false
    @State private var longPressed = false

    var body: some View {
        VStack(spacing: 20) {
            Text(tapped ? "Tapped!" : "Tap me")
                .onTapGesture {
                    tapped.toggle()
                }

            Text(longPressed ? "Long Pressed!" : "Long Press me")
                .onLongPressGesture(minimumDuration: 1.0) {
                    longPressed.toggle()
                }
        }
        .font(.title)
    }
}
```

> 📲 터치 인터랙션을 아주 간단하게 구성할 수 있음

---

## 🧪 예제 5: Drag 제스처 + 애니메이션

```swift
struct DragView: View {
    @State private var offset: CGSize = .zero

    var body: some View {
        Circle()
            .fill(Color.blue)
            .frame(width: 100, height: 100)
            .offset(offset)
            .gesture(
                DragGesture()
                    .onChanged { gesture in
                        offset = gesture.translation
                    }
                    .onEnded { _ in
                        withAnimation(.spring()) {
                            offset = .zero
                        }
                    }
            )
    }
}
```

> 🧲 드래그 중에는 실시간 위치 이동, 끝나면 스프링 애니메이션으로 원래 위치 복귀

---

## 🧪 예제 6: `matchedGeometryEffect`로 뷰 전환 연출

```swift
struct MatchedEffectView: View {
    @Namespace private var animation
    @State private var isExpanded = false

    var body: some View {
        VStack {
            if isExpanded {
                RoundedRectangle(cornerRadius: 20)
                    .fill(Color.purple)
                    .matchedGeometryEffect(id: "card", in: animation)
                    .frame(width: 300, height: 200)
            } else {
                RoundedRectangle(cornerRadius: 20)
                    .fill(Color.purple)
                    .matchedGeometryEffect(id: "card", in: animation)
                    .frame(width: 150, height: 100)
            }

            Button("Animate Card") {
                withAnimation(.spring(response: 0.5, dampingFraction: 0.7)) {
                    isExpanded.toggle()
                }
            }
        }
    }
}
```

> 💎 같은 ID를 공유한 뷰는 변화가 애니메이션으로 매끄럽게 연결됨

---

## ✅ 4단계 요약 체크리스트

* [x] `.animation()` 및 `withAnimation()` 사용법 숙지
* [x] `.transition()`으로 뷰 등장/제거에 애니메이션 적용
* [x] `TapGesture`, `LongPressGesture`, `DragGesture` 직접 사용
* [x] `matchedGeometryEffect`로 고급 전환 연출
* [x] 제스처와 애니메이션을 함께 사용하는 복합 인터랙션 구현

---

## 🛠 추천 연습 과제

* [ ] **카드 뒤집기 UI**: 버튼 탭 시 `.rotation3DEffect` 사용
* [ ] **드래그로 이동 후 되돌리기**: `DragGesture + withAnimation`
* [ ] **매치 효과로 카드 확대/축소 전환**: `matchedGeometryEffect` 활용

---

## 📚 추천 자료

* [Apple 공식 문서 - Animating Views](https://developer.apple.com/documentation/swiftui/animating-views-and-transitions)
* [SwiftUI Lab - Animations Deep Dive](https://swiftui-lab.com/swiftui-animations-part1/)
* [Hacking with Swift - Animations](https://www.hackingwithswift.com/quick-start/swiftui/animating-bindings)

---

## ⏭️ 다음 단계

[SwiftUI 5단계: 아키텍처와 앱 구조](SwiftUI_5.md)
