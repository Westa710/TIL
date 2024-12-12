# Today I learned

## Then()メソッド

- 非同期処理の結果をハンドリングするためのメソッド
- Promiseインスタンスの状態がsettled(fulfilled若しくはrejected)任あったとき実行するコールバックを登録する
- then()の戻り値は，登録したコールバックの戻り値で解決される新しいPromiseインスタンス

```Console
> const StringPromise = Promise.resolve('{"foo": 1}')
undefined
> stringPromise
Promise { '{"foo": 1}' }
> const numberPromise = stringPromise.then(str => str.length)
undefined
> numberPromise
Promise { 10 }
```

StringPromiseは値`'{"foo": 1}'`で解決され，`stringPromise.then(str => str.length)`により引数strにはこの値が入る．そして，コールバック関数の中身`str.length`が実行され，numberPromiseはコールバックからreturnされる文字列の長さ(＝10)を値として解決される．

### 例

```JavaScript
function parseJSONAsync(json) {
    return new Promise((resolve, reject) => 
        setTimeout(() => {
            try {
                resolve(JSON.parse(json))
            } catch (err) {
                reject(err)
            }
        })
    )
}
```

```console
# then()がfulfilledなPromiseインスタンスを返すパターン
> const stringPromise = Promise.resolve('{"foo": 1}')
undefined
> const objPromise = stringPromise.then(parseJSONAsync)
Promise { {"foo": 1} }
# then()がrejectedなPromiseインスタンスを返すパターン
> const rejectedObjPromise =
... Promise.resolve('不正なJSON').then(parseJSONAsync)
undefined
> (node:14088) UnhandledPromiseRejectionWarning: SyntaxError: Unexpected token 不 in JSON at position 0
...(省略)
```
