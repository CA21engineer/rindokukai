# 複雑になっているコードの例

```swift
private var isTextValid: Bool {
	 switch messageType {
	 case .text: return text != nil && text!.count <= 300 // 300 字以内
	 case .image: return text == nil || text!.count <= 80 // 80 字以内 or nil
	 case .official: return false // OfficialMessage はありえない
	}
}

private var isImageValid: Bool {
	return image != nil // image の場合だけ考慮する
}

var isValid: Bool {
	switch messageType {
	case .text: return isTextValid
	case .image: return isTextValid && isImageValid
	case .official: return false // OfficialMessage はありえない
	}
}
```

<img width="742" alt="スクリーンショット 2020-10-09 23 19 16" src="https://user-images.githubusercontent.com/41050625/96002446-136f7980-0e74-11eb-9a5c-fe94f494d826.png">


## 誤った抽象化よりも重複したコードの方がまし

ex. ~manager, ~service

[https://twitter.com/masuda220/status/937491771826741248?s=20](https://twitter.com/masuda220/status/937491771826741248?s=20)

# SOLID原則

## 単一責任

<img width="740" alt="スクリーンショット 2020-10-09 23 22 49" src="https://user-images.githubusercontent.com/41050625/96002541-30a44800-0e74-11eb-942d-402f0980d586.png">

### 手続き的凝集(Procedural Cohesion)

<img width="775" alt="スクリーンショット 2020-10-14 23 23 34" src="https://user-images.githubusercontent.com/41050625/96002690-60535000-0e74-11eb-9691-a323bb6bf35c.png">

ex. 

🙅‍♂️validationとか通信を含んだ1つのクラスが存在している状態

🙆‍♂️validationはMessageValidator class, 通信はそれ専用のクラスに分離している状態

### 論理的凝集(Logical Cohesion)

<img width="775" alt="スクリーンショット 2020-10-14 23 23 43" src="https://user-images.githubusercontent.com/41050625/96002751-6fd29900-0e74-11eb-9a2c-2b2eaa38ee5f.png">

🙅‍♂️

```swift
struct ImageMessageInputValidator {
	let image: UIImage?
	let text: String?
	var isValid: Bool {
		if image == nil { return false }
		if let text = text, text.count > 80 { return false }
		return true
	}
}
```

🙆‍♂️

```swift
struct ImageInputValidator {
	let image: UIImage?

	var isValid: Bool {
		guard let _ = image else { return false }
		return true
	}
}

struct MessageInputValidator {
    let text: String?

    var isValid: Bool {
        guard let text = text, text.count < 80 else { return false }
        return true
    }
}
```

### 失敗の可能性がある処理の表現

エラーが返ってきたことを

- `nil`で表現
- `isValid = false`で表現

本当に良いのか？

もちろんいい場合もあるけど、悪い場合もある

### Error Handling Rationale and Proposal

[apple/swift](https://github.com/apple/swift/blob/main/docs/ErrorHandlingRationale.rst)

[Swiftのエラー4分類が素晴らしすぎるのでみんなに知ってほしい - Qiita](https://qiita.com/koher/items/a7a12e7e18d2bb7d8c77)

<img width="757" alt="スクリーンショット 2020-10-09 23 41 04" src="https://user-images.githubusercontent.com/41050625/96002546-326e0b80-0e74-11eb-8f35-46b177b2255e.png">

最初の３つはまあ分かる

### Logic failure

preconditionとは？？

[Apple Developer Documentation](https://developer.apple.com/documentation/swift/1540960-precondition)

```swift
func foo(x: Int) {
  precondition(x >= 0) // falseの場合はランタイムでクラッシュする
  print(x) // `x` が 0 以上のときだけ実行される
}

foo(x: 0) // 0
foo(x: -1) // ランタイムエラー
```

[Swiftのpreconditionとassertの使い分け - Qiita](https://qiita.com/koher/items/ca7f388ab2a4e6747339)

因みに自分は使ったことがなく、Abemaでも使われていない

## 抽象に依存し、再利用性を向上する

<img width="775" alt="スクリーンショット 2020-10-14 23 25 48" src="https://user-images.githubusercontent.com/41050625/96003107-d8217a80-0e74-11eb-92ca-4430cea4ea86.png">

知らない人は↓を読んで

[Clean Architecture 達人に学ぶソフトウェアの構造と設計 (アスキードワンゴ)](https://www.amazon.co.jp/dp/B07FSBHS2V/ref=dp-kindle-redirect?_encoding=UTF8&btkr=1)

DIPを利用して、テストをしやすくする

### DI

<img width="775" alt="スクリーンショット 2020-10-14 23 25 59" src="https://user-images.githubusercontent.com/41050625/96003119-dbb50180-0e74-11eb-8dd9-86f877b29a6b.png">

### Method Injection

```swift
protocol MethodInjectable {
    associatedtype Configure
    func configure(_ configure: Configure)
}

class ViewController: UIViewController, MethodInjectable {
    typealias Configure = (text: String, color: UIColor)
    private var label: UILabel = UILabel()

		...

    func configure(_ configure: Configure) {
        label.text = configure.text
        label.textColor = configure.color
    }
}
```

### Setter Injection

### Constructor Injection

みんなが多分いつもやっているだろうパターン

### 病理学的結合

因みに、お互いがお互いのことを知っていることを、 `病理学的結合`って呼ぶらしい

## インターフェイス分離の法則

<img width="775" alt="スクリーンショット 2020-10-14 23 26 08" src="https://user-images.githubusercontent.com/41050625/96003127-dce62e80-0e74-11eb-9f42-da3d20b80c19.png">

↓を解決できる

<img width="775" alt="スクリーンショット 2020-10-14 23 26 16" src="https://user-images.githubusercontent.com/41050625/96003129-dd7ec500-0e74-11eb-8cb8-4212eb0b433f.png">

🙅‍♂️

```swift
protocol MessageSenderAPIProtocol {
    func fetchAll(ofUserId: Int, completion: ...)
    func fetch(id: Int, completion: ...)
    func sendTextMessage(text: String, completion: ...)
    func sendImageMessage(image: UIImage, text: String?, completion: ...)
}
```

🙆‍♂️

```swift
protocol MessageSenderAPIProtocol {
    func sendTextMessage(text: String, completion: ...)
    func sendImageMessage(image: UIImage, text: String?, completion: ...)
}

protocol MessageGetterAPIProtocol {
    func fetchAll(ofUserId: Int, completion: ...)
    func fetch(id: Int, completion: ...)
}
```

## 開放閉鎖の原則(Open/Closed Principle)

<img width="775" alt="スクリーンショット 2020-10-14 23 30 25" src="https://user-images.githubusercontent.com/41050625/96003499-3f3f2f00-0e75-11eb-8c75-ca0a4885784a.png">

SwiftではenumとかGenericsを使ってこれを表現できる

例は本見てね

# 幽霊型(Phantom Type)

同じ型なのに「ある状態でしか呼べないメソッド」を静的に実現できる設計パターン って本には書いてあった

```swift
protocol HungryState {}

// 幽霊型はインスタンス化する必要が無いのでenum
enum Hungry: HungryState {}
enum NotHungry: HungryState {}

struct Cat<State: HungryState> {}

extension Cat where State == Hungry {

    func eat() {
        print("I am hungry")
    }
}

extension Cat where State == NotHungry {

    func notEat() {
        print("I am not hungry")
    }
}

// 🙅‍♂️
// Cat<Hungry>().notEat()
// Cat<NotHungry>().eat()

// 🙆‍♂️
Cat<Hungry>().eat()
Cat<NotHungry>().notEat()
```

[Swiftで幽霊型を実装する - Qiita](https://qiita.com/shtnkgm/items/94938b39434cfa97e3f5)

終わり
