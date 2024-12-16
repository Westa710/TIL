# Today I Learned

## Promiseスタティックメソッドのショートサーキット

ショートサーキット・・・引数のすべてのPromiseインスタンスがPromiseインスタンスになるのを待たずに戻り値のPromiseインスタンスがsettledになること

| メソッド | ショートサーキットの条件 |
|-----|-----|
| Promise.all（） | 1つでもrejectedになったとき |
| Promise.race() | 1つでもsettledになったとき |
| Promise.allSettled() | ショートサーキットしない |
| Promise.any() | 1つでもfulfilledになったとき |

## Promiseの利用が適さないケース

Promiseインスタンスは一度settledになるとその後状態が変わらない．そのため，結果が複数に分割して返ってくるようなケース(サイズの大きいデータを分割して複数回にわたって送られるような場合など)ではPromiseを用いることは難しい
