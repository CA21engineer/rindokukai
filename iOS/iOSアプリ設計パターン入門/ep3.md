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