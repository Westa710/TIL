# Today I Learned

## async/await構文

- async，awaitというキーワードを用いて，非同期処理を簡潔に表現
- async関数の中でawaitというキーワードの後ろにPromiseインスタンスを渡す
- async関数は常にPromiseインスタンスを返す(関数内で非同期処理が実行されない場合でも)

ジェネレータ関数におけるyieldキーワードがawait相当
await後ろの非同期処理が終了するまで待機する．async関数の外部の処理はawaitの影響を受けない

### 例1

```Javascript
async function asyncFunc(input) {
  try {
    const result1 = await asyncFunc1(input)
    const result2 = await asyncFunc2(result1)
    const result3 = await asyncFunc3(result2)
    const result4 = await asyncFunc4(result3)
    // ...
  } catch (e) {
    // エラーハンドリング
  }
}
```

Promiseチェーンを使うとなれば，前段のthenの引数を次段のthenで参照することができず，ネストが深くなる
これを避けることがasync/await構文を使うメリットでもある

```JavaScript
function doSomethingAsync() {
  asyncFunc1(input)
    .then(result1 => asyncFunc2(result1)
      // result1を参照するためにPromiseインスタンスをネストする必要がある
      .then(result2 => asyncFunc3(result1, result2))
    )
}
```

## トップレベルawait

- CommonJS形式では，awaitキーワードは,async関数の中でしか使えない
- ESmodule形式では，トップレベルにawaitを記述してモジュールを非同期に読み込むことができる
- 時間のかかるモジュールの読み込みを行っている間に，他のモジュールを読み込みできる(ノンブロッキング処理)

### 例2

child-a.mjs

```JavaScript
console.log('child-a 1')
await new Promise(resolve => setTimeout(resolve, 1000))
console.log('child-a 2')
```

child-b.mjs

```JavaScript
console.log('child-b')
```

parent.mjs

```JavaScript
import './child-a.mjs'
import './child-a.mjs'
console.log('parent')
```

parent.mjsを実行すると出力は以下のようになる

```Console
$ node parent.mjs
child-a 1
child-b
 # 1秒待機
child-a 2
parent
```

CommonJS形式では，この対策として即時実行async関数式として非同期処理をする方法はある

```JavaScript
(async () => {
  // 非同期に値を取得
  const a = await getSomethingAsync()
})()
```
