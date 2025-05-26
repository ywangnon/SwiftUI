# ✅ SwiftUI 6단계: 비동기 처리와 네트워크 연동

## 🎯 학습 목표

* Swift의 `async/await` 문법 이해
* `URLSession`을 통한 REST API 호출
* API 응답 데이터 `Codable`로 디코딩
* 네트워크 에러 처리 및 로딩 상태 구현
* MVVM 패턴과의 통합

---

## 🧠 핵심 개념 요약

| 개념                | 설명                                |
| ----------------- | --------------------------------- |
| `async` / `await` | Swift의 비동기 처리를 위한 문법 (iOS 15+ 지원) |
| `URLSession`      | 네트워크 요청을 처리하는 프레임워크               |
| `Codable`         | JSON 데이터를 Swift 객체로 변환하는 프로토콜     |
| `Task`            | SwiftUI에서 async 함수를 호출할 때 사용      |
| `@MainActor`      | UI 관련 업데이트를 메인 스레드에서 수행           |

---

## 🧩 예시 앱 목표

**💡 간단한 뉴스 뷰어 앱 만들기**

* 외부 API에서 뉴스 리스트 가져오기
* 제목과 간단한 설명 표시
* 로딩 중 상태 표시
* MVVM 구조 적용

---

## ✅ 1. 뉴스 모델 정의

```swift
struct NewsArticle: Identifiable, Codable {
    let id = UUID()
    let title: String
    let description: String?
    
    enum CodingKeys: String, CodingKey {
        case title
        case description
    }
}
```

> ⚠️ 실제 API는 `UUID`가 없을 수 있으므로 `id`는 내부에서 생성
> `CodingKeys`를 이용해 Swift의 변수명과 JSON 키가 다를 경우 매핑

---

## ✅ 2. ViewModel 구성 (async/await 사용)

```swift
import Foundation

@MainActor
class NewsViewModel: ObservableObject {
    @Published var articles: [NewsArticle] = []
    @Published var isLoading: Bool = false
    @Published var errorMessage: String? = nil

    func fetchNews() async {
        isLoading = true
        errorMessage = nil

        defer { isLoading = false }

        let urlString = "https://newsapi.org/v2/top-headlines?country=us&apiKey=YOUR_API_KEY"
        guard let url = URL(string: urlString) else {
            errorMessage = "Invalid URL"
            return
        }

        do {
            let (data, _) = try await URLSession.shared.data(from: url)
            let decoded = try JSONDecoder().decode(NewsResponse.self, from: data)
            self.articles = decoded.articles
        } catch {
            errorMessage = "Failed to fetch articles: \(error.localizedDescription)"
        }
    }
}

struct NewsResponse: Codable {
    let articles: [NewsArticle]
}
```

> 📌 `@MainActor`는 UI 관련 속성들을 안전하게 메인 스레드에서 업데이트할 수 있도록 함

---

## ✅ 3. View 구성 (MVVM 연결)

```swift
import SwiftUI

struct NewsListView: View {
    @StateObject private var viewModel = NewsViewModel()

    var body: some View {
        NavigationStack {
            Group {
                if viewModel.isLoading {
                    ProgressView("Loading News...")
                } else if let error = viewModel.errorMessage {
                    Text("⚠️ \(error)")
                        .foregroundColor(.red)
                } else {
                    List(viewModel.articles) { article in
                        VStack(alignment: .leading, spacing: 8) {
                            Text(article.title)
                                .font(.headline)
                            if let desc = article.description {
                                Text(desc)
                                    .font(.subheadline)
                                    .foregroundColor(.secondary)
                            }
                        }
                        .padding(.vertical, 4)
                    }
                }
            }
            .navigationTitle("Top News")
            .task {
                await viewModel.fetchNews()
            }
        }
    }
}
```

> 🟡 `.task`는 뷰가 나타날 때 `async` 함수 호출에 사용
> 🔄 `ProgressView`, `errorMessage`, `articles` 상태에 따라 UI 분기

---

## ✅ 4. 상태 흐름 요약

```text
🟢 사용자 진입
→ .task 호출
→ ViewModel에서 fetchNews() async 시작
→ URLSession으로 JSON 요청
→ 성공 시: articles 업데이트 → 화면 갱신
→ 실패 시: errorMessage 표시
```

---

## ✅ 보너스: 비동기 이미지 로딩 (AsyncImage)

```swift
AsyncImage(url: URL(string: "https://example.com/image.png")) { phase in
    switch phase {
    case .empty:
        ProgressView()
    case .success(let image):
        image
            .resizable()
            .scaledToFit()
    case .failure:
        Image(systemName: "photo")
    @unknown default:
        EmptyView()
    }
}
.frame(height: 200)
```

> 📷 `AsyncImage`는 비동기 이미지 로딩을 매우 쉽게 처리할 수 있게 해줍니다 (iOS 15+)

---

## ✅ 6단계 요약 체크리스트

* [x] `async/await`와 `Task`의 동작 방식 이해
* [x] `URLSession`으로 비동기 네트워크 통신 구현
* [x] `Codable`을 통해 JSON → 모델 변환
* [x] 에러 메시지 및 로딩 상태 UI 처리
* [x] MVVM 구조에 비동기 통신 통합

---

## 🛠 추천 실습 과제

* [ ] 날씨 앱: OpenWeather API를 사용해 현재 날씨 표시
* [ ] 영화 앱: TMDB API를 사용해 영화 리스트 불러오기
* [ ] Github 사용자 검색: 검색어에 따라 사용자 목록 호출

---

## 📚 추천 자료

* [Apple 공식 문서 – URLSession](https://developer.apple.com/documentation/foundation/urlsession)
* [Swift Async/Await 개념 정리 – WWDC21](https://developer.apple.com/videos/play/wwdc2021/10132/)
* [News API](https://newsapi.org) – 무료 뉴스 데이터 제공
