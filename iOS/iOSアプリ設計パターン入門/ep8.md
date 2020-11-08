# Flux

## Flux アーキテクチャの概要

Flux アーキテクチャは、`ラテン語の Fluxus` という単語から名付けられています。
Fluxusは `Flow(流れ)`を意味しており、その名のとおり Flux アーキテクチャでは データフローが**単一方向**であることが中心の考えになっています。

<img width="694" alt="スクリーンショット 2020-11-08 12 47 27" src="https://user-images.githubusercontent.com/41050625/98456408-90b2b380-21c0-11eb-9c40-2bd4b54274b4.png">
<img width="774" alt="スクリーンショット 2020-11-08 12 46 47" src="https://user-images.githubusercontent.com/41050625/98456396-78db2f80-21c0-11eb-921e-9f5a909c89d3.png">

## Viewの構成とデータフロー

Flux アーキテクチャでは**状態を管理**するのは Store の役割であるため、View が状態を持つことはありません。
View へ向かうデータフローは、Store の状態を View に**反映**するデータフローとなります。iOS アプリでは `NotificationCenter の通知機能`や `Observer パターンを担うライブラリ(RxSwift、ReactiveCocoa など)`などでデータフローを実現できます。

<img width="772" alt="スクリーンショット 2020-11-08 12 54 58" src="https://user-images.githubusercontent.com/41050625/98456477-9e1c6d80-21c1-11eb-8ef0-815a451f9d14.png">
<img width="753" alt="スクリーンショット 2020-11-08 12 55 44" src="https://user-images.githubusercontent.com/41050625/98456485-b8564b80-21c1-11eb-9a33-4ab31e7342f0.png">

## Fluxのメリデメ

<img width="759" alt="スクリーンショット 2020-11-08 14 36 00" src="https://user-images.githubusercontent.com/41050625/98458026-bb583880-21cf-11eb-8566-96c856fde9f5.png">

## Actionの定義

以下、GitHub のリポジトリ検索を例とする

Action は、「Store が Action を受け取った際に処理をすべきかを判断するための `type`」と「 type に紐づく `data`」で構成されます。

enumを使うと、以下の様に定義できる

```swift
enum Action {
  case addRepositories([GitHub.Repository])
  case clearRepositories
}
```

```swift
switch action {
  case let .addRepositories(repositories):
  // 配列を追加する処理
  case .clearRepositories:
  // 配列をクリアする処理
}
```

## ActionCreater

```ActionCreater.swift
final class ActionCreator {
  private let dispatcher: Dispatcher
  private let apiSession: GitHubApiRequestable

  init(
    dispatcher: Dispatcher = .shared,
    apiSession: GitHubApiRequestable = GitHubApiSession.shared
  ) {
    self.dispatcher = dispatcher
    self.apiSession = apiSession
  }

  func searchRepositories(query: String, page: Int) {
    apiSession.searchRepositories(query: query, page: page) { [dispatcher] result in
      switch result {
      case let .success(repositories, _):
        dispatcher.dispatch(.addRepositories(repositories))
      case let .failure(error):
        print(error)
      }
    }
  }

  func clearRepositories() {
    dispatcher.dispatch(.clearRepositories)
  }
}
```

## Dispatcher

コードは省略mm

Dispatcher は、1つのコンテキストに対して1つだけ存在するものであるため、`シングルトン`を定義します。アプリケーションコードで、ActionCreator や Store からDispatcher を利用する場合は、シングルトンを利用する。  ###
一方でテストコードでは、イニシャライザを利用してインスタンス化したDispatcherを利用する

## Store

Store は、複数存在することがあります。Store を分ける単位は、画面ごとであったり、役割ごとであったりします。  
だが、個人的にはStoreも画面間を跨いで使うものがあるので、基本的にはシングルトンでは？と個人的には思っている。  

## Fluxを提供するライブラリ

- https://github.com/ReactorKit/ReactorKit
- https://github.com/ra1028/VueFlux
