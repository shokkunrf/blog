---
title: "TypeScript v4 + React v17 で Emotion v11 を動かしてみる"
date: 2021-01-31T23:03:49+09:00
draft: false
---

Emotion v11 の導入につまずいたので残しておく。

## 今回の環境

- TypeScript: 4.1.3
- React: 17.0.1
- react-script: 4.0.1
- @emotion/react: 11.1.4

## つまづきから解決まで

### cra で React のひな形を生成する

次のコマンドで facebook が提供している TS を使った React のテンプレートを生成する。

```sh
npx create-react-app myapp --template typescript
```

App.tsx を次のように変更する。 (この記事のために簡素にする)

```tsx:App.tsx
import React, { FC } from 'react';

const App: FC = () => (
  <div>
    red
  </div>
);

export default App;
```

### つまづき

emotion v11 の React 用パッケージ`@emotion/react`をインストール。

```sh
npm install @emotion/react
```

tsconfig.json に以下を追加し、React の jsx ではなく emotion の jsx を使うように設定する。

```json:tsconfig.json
"compilerOptions": {
  "jsxImportSource": "@emotion/react"
}
```

App.tsx にスタイルを追加する。

```tsx:App.tsx
import React, { FC } from 'react';
import { css } from '@emotion/react';

const myStyle = css`
  color: red;
`;

const App: FC = () => (
  <div css={myStyle}>
    red
  </div>
);

export default App;
```

これでコンパイルは通るがなぜかスタイルが適用されない。\
生成された HTML を見てみると css props が動かないよ と書いてある。

```html
<div
  css="You have tried to stringify object returned from `css` function. It isn't supposed to be used directly (e.g. as value of the `className` prop), but rather handed to emotion so it can handle it (e.g. as value of `css` prop)."
>
  red
</div>
```

### 解決方法 1

[公式](https://emotion.sh/docs/css-prop#jsx-pragma)によると、
cra4 以降を使ったアプリケーションでは以下の JSX Pragma を使わなくてならない
とのこと。\
また、emotion の jsx を JSX Pragma で設定しているため、上記でした`tsconfig.json`の変更は必要ない。

```tsx:App.tsx
/** @jsxImportSource @emotion/react */
import React, { FC } from 'react';
import { css } from '@emotion/react';
const style = css`
  color: blue;
`;

const App: FC = () => (
  <div css={style}>
    blue
  </div>
);

export default App;
```

### 解決方法 2

[このブログ](https://www.atnr.net/emotion-react-css-prop-error/)によると、以下の JSX Pragma を記述しても動作する。\
ただし、この場合は`tsconfig.json`に以下を追加しなくてはならなず、また、`@emotion/react`から`jsx`をインポートしなくてはならない。

```json:tsconfig.json
  "compilerOptions": {
    "types": ["@emotion/react/types/css-prop"]
  }
```

```tsx:App.tsx
/** @jsxRuntime classic */
/** @jsx jsx */

import React, { FC } from 'react';
import { jsx, css } from '@emotion/react';
const style = css`
  color: blue;
`;

const App: FC = () => (
  <div css={style}>
    blue
  </div>
);

export default App;
```

### (解決できなかった方法)

解決方法 1 でも 解決方法 2 でも JSX Pragma が必要であり、CSS を書くたびにコピペするのは面倒。\
そう考える場合、[ここ](https://emotion.sh/docs/css-prop#babel-preset)にあるように`.babelrc`に追記することで、コンパイル時に emotion の JSX を参照するように設定できるらしい。\
ただ、今回の構成では cra(react-script)を利用しており、`.babelrc`を追加しても無視されてしまう。\
そのため、これを利用するには eject して react-script をやめるしかない。\
実際に eject して試してみたが babel や webpack に関する知識が不足していたためスタイルの適用ができなかった。\
また、[ここ](https://emotion.sh/docs/install)に cra を使ってるなら[`Babel macro`](https://emotion.sh/docs/babel-macros)が使えるよ って書いてあるけど、cra v2 でないとダメみたい。

## おわりに

今後の更新によっては設定が不要になるかもしれないし、また大きく書き方が変わるかもしれない。\
どちらにせよ、JS を取り巻く環境であったり、JSX Pragma であったりは理解しておいたほうが良いと感じた。\
Babel や webpack などのなんとなく避けていたものに目を向けなくてはなー。\
ちなみに、css prop を使うのはプレゼンテーションコンポーネントに限られるから JSX Pragma を書いてもいいかなと思うので、私は解決方法 1 を使ってます。

## 参考資料

- [Emotion](https://emotion.sh)
- [Emotion + React で css プロップがないというエラーが出たときの対策 - atnr.net](https://www.atnr.net/emotion-react-css-prop-error/)
