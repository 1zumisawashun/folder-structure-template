# folder-structure-template

- React・Next.js のディレクトリ構成の骨子をまとめたリポジトリです
- スタイルは CSS Modules を想定しています

## Overview

- [React](https://github.com/1zumisawashun/folder-structure-template/blob/main/react-vite/README.md)

- [Next.js Page Router](https://github.com/1zumisawashun/folder-structure-template/blob/main/nextjs-page-router/README.md)

- [Next.js App Router](https://github.com/1zumisawashun/folder-structure-template/blob/main/nextjs-app-router/README.md)

## assets

```
assets/
├─ images/
│   └─ ...
└─ styles/
    ├─ components/
    ├─ elements/
    ├─ generics/
    ├─ objects/
    ├─ settings/
    ├─ tools/
    ├─ utilities/
    └─ app.css
```

- ITCSS のガイドラインは[こちら](https://github.com/1zumisawashun/sass-template)

## components

```
components/
├─ buttons/
│  └─ Button
│     ├─ index.module.css
│     ├─ index.stories.tsx
│     ├─ index.test.tsx
│     └─ index.tsx
├─ elements/
│  └─ Dialog
│     ├─ index.module.css
│     ├─ index.stories.tsx
│     ├─ index.test.tsx
│     └─ index.tsx
├─ forms/
│  ├─ Checkbox/
│  ├─ TextInput/
│  ├─ Select/
│  └─ ...
└─ layouts/
      └─ VStack
         ├─ index.module.css
         ├─ index.stories.tsx
         ├─ index.test.tsx
         └─ index.tsx
```

## functions

```
functions/
├─ constants/
├─ helpers/
├─ hooks/
│     └─ useDisclosure/
│        ├─ index.test.ts
│        └─ index.ts
├─ libs/
└─ types/
```

## features

```
features/
├─ todo/
│  ├─ TodoCreate/
│  ├─ TodoEdit/
│  ├─ TodoDetail/
│  ├─ TodoList/
│  │  ├─ components/
│  │  ├─ hooks/
│  │  └─ index.tsx
│  │
│  ├─ todo.schema.ts
│  └─ todo.type.ts
│
└─...
```

## Troubleshoot

<details>
<summary>featuresディレクトリに格納するか迷った時</summary>

- layer か feature か、どちらに格納するかはトレードオフなので正解はない
- これは案件のフェーズやサイズにもよる
  - 個人的観測としてスモール〜ミドルサイズは layer がやりやすい、ミドル〜ラージサイズは feature が管理しやすい

</details>

<details>
<summary>ドメインを持つコンポーネントを別のドメインで使いたい</summary>

- 具体的にはプロダクトのドメインがある ProductCard コンポーネントをマイページのドメインでも使用したい場合
- 以下添付画像のように features/product/components/ProductCard に格納する（components/elements には格納しない）
- features/ドメイン/〇〇のようにドメイン直下は別ドメイン or 同ドメインかつ別ページでも使用される
- 例えば ProductForm は ProductCreate と ProductEdit で使われているので features/product/components/に格納している
- hooks や型定義も同様にドメイン直下に配置して暗黙的に使いまわされることを明示する
- テストを書くのもここに集中させればよくないか？

<img width="374" alt="image" src="https://github.com/1zumisawashun/unifree-client/assets/65071534/c9034c1e-e6b8-459e-ac70-1ce6874d5e78">

</details>

## Next.js App Router

### フェッチ戦略

以下で統一する

- (pages)ディレクトリでフェッチをする
- component level fetch は実施しない
- (pages)ディレクトリでフェッチした値を props で小コンポーネントに流す

※従来の Page Router との差分を無くし認知負荷を下げる意図がある + App Router に依存する箇所を(pages)ディレクトリに集約することで、万が一 App Router を剥がす時に容易にする

### 注意点

- 本来であれば「自律分散なデータ取得」が App Router の基本になるので従来の Page Router（gssp）のデータ取得はアンチパターンかもしれない

### 「自律分散なデータ取得」のメリット

> 非同期コンポーネントにすることで Streaming SSR + Suspense ができる

### トラブルシューティング

- 実装上どうしても(pages)ディレクトリ以外でフェッチをしなくてはいけない時は？
  - Presentation / Container パターンを使う
