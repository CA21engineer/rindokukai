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

# 第７回 CyberAgent21 卒内定者 iOS 輪読会

### 第 9 章 Redux

---

# 第 9 章 Redux

---

## Redux とは - 背景 -

- Flux と Elm に影響を受けて 2015 年 8 月 にリリースされたアーキテクチャ
  - Flux のアイデア
  - Elm の複雑性に対するアプローチ
    - 副作用を暗黙に生じさせない

---

## Redux の生まれた目的

### 課題

- アプリケーションの状態の更新の把握が困難
  - 変化 (mutation) と非同期性 (asynchronicity) を金剛しているのが根源

### 解決策

- Flux のように情報の伝搬する方向を一方向にする
- いつどのような形で更新が起こるのかを明瞭にする
- 純粋関数による副作用の排除
- イミュータブルな状態表現の制約

---

## Redux とは - アーキテクチャ概要 -

- Action
  - Redux レイヤーに対して任意のビジネスロジックの実行や状態の変更を依頼するためのメッセージ
- State
  - アプリケーションの状態を表現するデータの集合
- Reducer
  - Action と現在の State を引数にとって新しい State を出力する関数
- Store
  - State と Reducer を保持するアプリケーション内で単一のインスタンス。Action の dispatch、Reducer の実行、View からの State の購読の機能を持つ

---

## Redux とは - ３つの原則 -

予測可能な形でコードを構造化するための骨組

- 信頼できる一意となる状態を唯一とする (Single source of truth)
- 状態はイミュータブルで表現する (State is read-only)
- 状態の変更は純粋関数で記述する (Changes are made with pure functions)

---

## 信頼できる一意となる状態を唯一とする

- アプリケーション全体の状態を単一のオブジェクトツリーで表現する
  - 開発時のデバッグが容易になる(? そうなんですね)

---

## State はイミュータブルで表現する

- Reducer により新しい State が生成されるまで State は変更されないインスタンスである

---

## State の変更は純粋関数で記述する

- 関数実行時に副作用を発生させない関数で記述

### 副作用とは？

- 関数に入力されていない要素が出力とは関係ない箇所で変化すること

### 純粋関数の特性

- 与えられた要素や関数外の要素を変化させず、戻り値以外の出力を行わない
- 取り扱う全ての要素が引数として宣言されている
- 入力に対して出力が常に一意である

---

## State の変更は純粋関数で記述する

### iOS アプリの全てのコードを純粋関数で書くことは可能か？

- 不可能
  - UIKit と Swift の特性から困難
  - 住み分けをしてあげる必要がある

---

## State

- struct のツリー構造で構成
- 最上位の State を `AppState` として、直下に各画面の State を配置

---

## Action

- enum または struct で表現

---

## ActionCreator

- 副作用が許容
- 出力は `Optional<Action>` のため、 Action を作らずとも良い

---

## Reducer

- State のツリー構造の各ノードに対して１つずつ Reducer を配置する
- Store は最上層の Reducer のみ参照
  - 上層の Reducer が下層の Reducer を評価していることで構成

---

## Store

- State と Reducer を保持し、Action をディスパッチする
- 購読機能を有する
