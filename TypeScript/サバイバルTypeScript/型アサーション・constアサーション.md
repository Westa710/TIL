# 型アサーション・constアサーション

## asキーワードによる型アサーション

`as`キーワードを用いて，型推論を上書きする

例：`string | number`型を`string`型にアサーションしている．

```typescript
const value: string | number = "this is a string";
const strLength: number = (value as string).length;
console.log(strLength);
// 16
```

`number`型を`string`型に型アサーションするような，お互いの型に共通する部分が少ないと型アサーションすることができない．

型アサーションはコンパイラーの型推論を上書きする強力な機能なので，むやみやたらに使うべきではない．型ガード関数を作るなどで型の絞り込みを行い，型アサーションを防ぐ方法を検討するべき．

## constアサーション

変数宣言の時に末尾に`as const`キーワードをつけると，その値をreadonlyにしたうえでリテラル型にする．

```typescript

const array = [1, 2, 3] as const;
const obj = {
  name: "Alice",
  age: "20",
} as const;

// obj.name = 30;
// 読み取り専用プロパティであるため、'name' に代入することはできません。

const obj2 = {
  name: "Bob",
  health: {
    weight: 50,
    height: 180
  }
} as const;

// obj2.health.weight = 40;
// 読み取り専用プロパティであるため、'weight' に代入することはできません．
```

オブジェクトの中にオブジェクトがある場合は，深い階層にあるプロパティまでreadonlyにする．一方，`readonly`キーワードであるオブジェクトを読み取り専用にすると，そのオブジェクトのオブジェクトはreadonlyにならず，書き換えることができる．

