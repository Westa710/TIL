# undefinedとnullの違い

言語仕様上の違いは明確であるが，
意味合いの違いとしては，undefinedが「値が代入されていないため，
値がない」，nullは「代入すべき値が存在しないため，値がない」といった漠然とした違い．

- nullは自然発生しない
undefinedは明示的に用いなくても自然に発生する．
例えば，オブジェクトに存在しないプロパティや配列にない要素にアクセスしたときなど
一方，JavaScriptとしてはnullを提供することはない(一部のAPIを除いて)

- undefinedは変数，nullはリテラル
undefinedもnullもプリミティブ型の値ではあるが，undefinedは変数であり，nullはリテラルである．そのため，nullという名前の変数を作ることはできない．

## 使い分け

コードの各所でどちらかを使うべきか考えるのは，それに必要な労力が
大きい割にメリットが少ないので基本的にはundefinedを使うのがよさげ．

## 補足

- 値の有効性を表す意味でundefinedを使用しない
  
以下は悪い例

```typescript
function toInt(str:string) {
  return str ? parseInt(str) : undefined;
}
```

入力された値が有効かどうかを示す属性を持たせるべき．

```typescript
function toInt(str: string): { valid: boolean, int?: number } {
  const int = parseInt(str);
  if (isNaN(int)) {
    return { valid: false };
  }
  else {
    return { valid: true, int };
  }
}
```

出典：[TypeScript Deep Dive](https://typescript-jp.gitbook.io/deep-dive/recap/null-undefined)
