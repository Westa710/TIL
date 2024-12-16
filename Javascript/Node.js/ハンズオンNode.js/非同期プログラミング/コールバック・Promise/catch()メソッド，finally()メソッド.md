# Today I Learned

## catch()メソッド

- 非同期処理の結果をハンドリングするためのインスタンスメソッド
- then()の引数に渡すonFulfilled, onRejectedのうちonFulfilledを省略したもの

```console
# then()でonFulfilled()を省略
> const withoutOnFulfilled =
... Promise.reject(new Error('エラー')).then(undefined, () => 0)
undefined
> withoutOnFulfilled
Promise { 0 }

# 同じ処理をcatch()で記述
> const catchedPromise = Promise.reject(new Error('エラー')).catch(() => 0)
undefined
> catchedPromise
Promise { 0 }
```

### then()と何が違う？

then()を2引数(onFulfilled, onRejected)で実行する場合は,onFulfilledの中で発生したエラーはonRejectedではハンドリングされない．一方，then().catch()のように記述することで，then()のonFulfilledのなかで発生したエラーをハンドリングすることができる．

***

## finally()メソッド

- try...catch構文におけるfinallyブロック相当の機能
- 非同期処理が成功したかどうかにかかわらず，Promiseがsettledになったときに実行されるコールバックを登録
- finally()に登録するコールバックは，引数無し

```console
> const onFinally = () => console.log('finallyのコールバック')
undefined
> Promise.resolve().finally(onFinally)
Promise { <pending> }
finallyのコールバック

> Promise.reject(new Error('エラー')).finally(onFinally)
Promise { <pending> }
finallyのコールバック
  # 以下, UnhandledPromiseRejectionWarning
```

then(),catch()と異なり，finally()の戻り値はPromiseが解決される値に影響を及ぼさない．一方エラーが発生した場合は，finally()の返すPromiseも同じ理由で拒否される．

## then(), catch(), finally()のコールバックの実行タイミング

これらのメソッドに渡すコールバックは，各コールバックがPromiseインスタンスを返してさえいればPromiseの状態にかかわらず常に非同期的に実行される．
