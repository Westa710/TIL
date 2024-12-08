#　Today I learned...
## プロトタイプチェーン

- JavaScriptのクラスは，プロトタイプチェーンに基づく継承の仕組みを持っている．
- クラスに定義したメソッド，コンストラクタ，getter，setterはそのクラスの`prototype`に追加される．
- クラスからインスタンスを生成すると，prototypeはインスタンスの`__proto__`プロパティにセットされる．

```javascript

class Foo {
...
# getter, setter, constructorなど
...
}

new fooInstance = new Foo(...)

# インスタンスの__proto__はクラスのprototypeと等しい
$ fooInstance.__proto__ === Foo.prototype
true

```

***

## CommonJSモジュール

- Code.jsはCommonJSモジュールとESモジュールに対応しているが，ESモジュールは実験的な機能段階
- モジュール側は`module.exportsを通じて外部に関数や変数を宣言
- ロードする側は`require()`関数を用いる．JSONファイルもロード可能
- Node.jsコアのモジュールはREPL(対話型環境)においては明示的なrequireは不要

cjs-math.js

```javascript
module.exports.add = (a, b) => a + b
```

コンソール

```console
> const math = require('./cjs.math')
undefined
> math.add(1, 2)
3
```

***

## strictモード

- モジュールの先頭に'use strict'と記述することでstrictモードが有効に
- 安全でパフォーマンスに優れたモード

## strictモードでない場合

cjs-non-strict-mode.js

```javascript
let myString = 'いろは'
//変数のタイプミス
myStrng = 'にほへと'
console.log(global.myStrng)
```

コンソール

```
$ node cjs-non-strict-mode
にほへと
```

***

## strictモードの場合

cjs-strict-mode.js

```javascript
'use strict'
let myString = 'いろは'
//変数のタイプミス
myStrng = 'にほへと'
console.log(global.myStrng)
```

コンソール

```
$ node cjs-strict-mode
ReferenceError: myStrng is not defined
...
```
