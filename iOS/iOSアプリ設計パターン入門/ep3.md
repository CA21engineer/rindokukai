---
marp: true
header: "**CyberAgent21卒内定者iOS輪読会** - iOSアプリ設計パターン入門"
footer: "by **＠ry_itto**"
theme: default
paginate: true
style: |
    section.title * , h1{
        text-align: center;
    }
---


 # 第三回 CyberAgent21卒内定者iOS輪読会

 ### 第5章 MVC (p83 - p94)
 ### 第6章 MVP (p95 - p110)

 ---

# MVC

---

## MVC とは

プログラムを

- 入力
- 出力
- データの処理

に分けたとき

---

## MVC とは

以下3つと定義された構成のアーキテクチャのこと

- 入力
    - Controller
- 出力
    - View
- データの処理
    - Model

---

## MVC とは

### アプリケーションの処理から入力・出力を分離
### -> データの処理そのものに専念しやすくなる

---

## 原初 MVC

以下三つのデザインパターンを中軸に構成

- Composite
- Strategy
- Observer

---

## 原初 MVC

- View
    - Observer パターンを使用して Model の状態を監視
        - 監視している Model の状態に応じて自身の描画
    - Strategy パターンを使用して 適切な Controller を生成
- Controller
    - 入力を受けて Model に処理を依頼
- Model
    - 依頼を受けた際に処理を行い、自身の状態を更新

---

## 原初 MVC 補足

### Composite パターン
> Composite パターンを用いるとディレクトリとファイルなどのような、木構造を伴う再帰的なデータ構造を表すことができる。
> [Composite パターン - Wikipedia](https://ja.wikipedia.org/wiki/Composite_%E3%83%91%E3%82%BF%E3%83%BC%E3%83%B3)

- ある View が自身の必要な View を持ち、その子の View もまた必要な子を再帰的に持っている

---

## 原初 MVC 補足
### Strategy パターン
> Strategy パターンは、コンピュータープログラミングの領域において、アルゴリズムを実行時に選択することができるデザインパターンである。
> [Strategy パターン - Wikipedia](https://ja.wikipedia.org/wiki/Strategy_%E3%83%91%E3%82%BF%E3%83%BC%E3%83%B3)

Java をやった時とかに話に出てくるポリモーフィズムと同じ効果を持っていそう。
（というか、 Java の例だと `ポリモーフィズムを使って Strategy パターンを実現することができる` とあるので本質的には違うかもしれない。

---

## 原初 MVC 補足
### Observer パターン
> 観察される側(=Subject)と観察する側(=Observer)の2つの役割が存在し、Subjectの状態が変化した際にObserverに通知されるデザインパターンです。そのため、状態変化に応じた処理を記述する時に有効です。
> [デザインパターン「Observer」by @shoheiyokoyama](https://qiita.com/shoheiyokoyama/items/d4b844ed29f84a80795b)
---

# MVP

---

## MVP とは

- 画面の描画処理とプレゼンテーションロジックを分離
- 目的
    - テスト容易性・作業分担のしやすさ
        - 保守しやすい
        - MVC の責務の再分割
    - Presenter が View に対して手続き的に描画指示を出す「フロー同期」を導入

---

## データの同期方法

- フロー同期
- オブザーバー同期

---

## データの同期方法
### フロー同期

- 上位のレイヤーのオブジェクトから下位のレイヤーのオブジェクトに対して都度データを同期する方法 (手続き的な方法)

### オブザーバー同期

- 下位のレイヤーのオブジェクトから上位のレイヤーのオブジェクトをオブザーバパターンを使用して監視し、イベント通知を受け取ってデータを同期する方法 (宣言的な方法)

---

## データの同期方法
### フロー同期
#### メリット
- データの流れを追いやすい

#### デメリット
- データを使用している全てのオブジェクトの参照を所持していなくてはいけないため、参照の管理が大変

---

## データの同期方法
### オブザーバー同期
#### メリット
- データを複数の箇所で監視しやすい

#### デメリット
- データが変更されるたびに同期処理が走ってしまうため、データの流れがフロー同期に比べて追いにくい

---

## データ同期方法
### iOS アプリ開発での具体的な例
#### フロー同期

```swift
class HogePresenter {
    let view: UIView

    ...

    func update() {
        view.label.text = 'hogehoge'
    }
}
```

---

## データ同期方法
### iOS アプリ開発での具体的な例
#### オブザーバー同期

```swift
class HogePresenter {
    let titleSubject: PublishSubject<String>
}
class HogeViewController {
    let presenter: HogePresenter
    @override
    func viewDidload() {
        presenter.titleSubject.asObservable()
            .bind(to: label.text)
    }
}
```

---

## MVP の構造

### 共通

#### Model
基本的に　MVC と同じ

---
## MVP の構造

### 共通

#### View 
- ユーザー操作の受付・画面表示を担当
- iOS での MVP では　ViewController も View に含む
- Model の処理読んだり
- Model の変更があれば表示を変更
    - Model の変更の伝搬方法の違い
        - `Passive View`
        - `Supervising Controller`
    
---
## MVP の構造

### 共通

#### Presenter 
- プレゼンテーションロジックを担う
- Model に画面表示に関わるロジックを持たせたくない時に使用
- 1View 1Presenter
    
---

## MVP の構造

### Passive View (Passive -> 受動的)
- View を完全に受け身にするパターン
- View は全てのユーザーからの入力を Presenter に丸投げ
- データのやりとりはフロー同期を用いる
- プレゼンテーションロジックのテストがしやすくなる
- Model へは Presenter からのみアクセス
    
---

## MVP の構造
### Supervising Controller

- フロー同期・オブザーバー同期の両方を使うパターン
- View: 
    - Presenter とフロー同期
    - Model と オブザーバー同期
- View は全てのユーザーからの入力を Presenter に丸投げ
- View は簡単なプレゼンテーションロジックをもつ
- Passive View のもつ冗長さの解決

---

## 2つの MVP の選定基準
- すべてのプレゼンテーションロジックをテスト (筆者の基準)
    - したい
        - Passive View
    - したくない
        - Supervising Controller

---

## 実装例はサンプルコードへ 05, 06
https://github.com/peaks-cc/iOS_architecture_samplecode