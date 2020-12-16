---
marp: true
header: "**CyberAgent21卒内定者iOS輪読会** - iOSアプリ設計パターン入門"
footer: "by **＠ostk0069**"
theme: default
paginate: true
style: |
    section.title * , h1{
        text-align: center;
    }
---

# 第11回　CyberAgent21卒内定者iOS輪読会

---

# 今回は番外編です

# The Composable Architecture について行います

---

# 今回の話では The Composable Architectureは長いので本家でもよく出てくるTCAとして記すこととします

---

### 発表者はTCAについてプロジェクトととしての運用経験はないのでマサカリをする際には十分に配慮した上で行っていただけると助かります

---

## 今回話すこと

- 理解する上で必要な知識
- 主な概要や誕生経緯
- 5つの特徴
- 登場するClassの役割
- 導入することのメリット・デメリット
- サンプルコードを見る

---

## 今回話さないこと

- 長期的に使うことのメリデメ
- 細かい圏論的な話

僕の経験はry-ittoとハッカソンの時に使ったことがあるぐらいです

--- 

# 理解する上で必要な知識

---

## 必要な知識
- Combine(UIKit+RxSwiftもある一応ある)
- DI
- iOSで用いる設計ほぼ全て
    - 特にReduxやFlux

---

## あると理解がはやいもの
- Elm Architecture
- Vuex

比較的考えが似ている、または開発者がそれにinspireされて作ってる

---

# 主な概要や誕生経緯

---

## 二人の有名な開発者によって作られた

- Brandon Williams
    - Kick Starter(iOS), Kick Starter(Android)など

- Stephen Celis
    - Kick Starter(iOS), Snapshot Testingなど

---

## 動画が多数存在するのでとりあえず見てない人は見た方がいいかも
- https://www.pointfree.co/collections/composable-architecture/a-tour-of-the-composable-architecture/ep100-a-tour-of-the-composable-architecture-part-1

TCA法海以外にも色々考え方を学べそうなコンテンツ(英語レベル高め)

---

生まれた経緯を話す前にiOSの設計の歴史を振り返りましょう(時期は大まかに)

---



---

生まれた経緯
- SwiftUIが誕生したから！ではない

---

# 5つの特徴

---

- State Management
- Composition
- Side Effect
- Testable
- Ergonomics

---

## State Management
- 状態管理をReduxやFluxのように行う

---

## Composition
- コンポーネント思考

---

## Side Effect
- TCAが広まる１番の要因
- サービスにおける副作用に耐えうる設計になってる
- CombineのPublisherに準拠してる

---

## Testable
- 書くべきテストコードの明確化
- 

---

# Ergonomics
- 簡略化

---

# 登場するClassの役割

---

# 導入することのメリット・デメリット

---

# メリット
- 状態管理の明確な階層分け
    - 設計に関する議論がチームで行いやすくなる
- modular architectureの適用のしやすさ
- 採用につながる

---

# デメリット
- ライブラリとしての依存をしないといけない(自作でも問題ない)
    - CombineをラップしてるのでCombineへの理解とiOS13以上ではないと使えない
        - UIKitのサンプルも存在する
- 別の設計への乗り換えはそこそこしんどそう
- 長期プロジェクトになることが決まってないサービス
- チームのレベル感次第では崩壊する
    - 特性を理解せずに導入するにはむしろマイナスが発生する可能性も

---

- AndroidにTCAは？
    - あると面白いけど邪魔の可能性が高そう

---

# サンプルコードを見る

---

- counter demo
    - Effectは関係ない、行ってしまえばもはやTCAではない

- effect basic
    - counter demoに変な仕様を追加することでeffectの良さを知ることができる

---

# 質問 & 議論

---

# 次回は 第13章 & CAの設計 です！