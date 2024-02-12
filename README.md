# folder-structure-template

folder-structure-template

## Overview

- React・Next.js のディレクトリ構成の骨子をまとめる
- Next.js は Page Router と App Router 両方とも想定して作成した
- 基本的には package by feature で構成している
- React・Next.js のディレクトリ構成をできるだけ似せて認知負荷を避けている

## FolderStructure

- [React](https://github.com/1zumisawashun/folder-structure-template/blob/main/react-vite/README.md)

- [Next.js Page Router](https://github.com/1zumisawashun/folder-structure-template/blob/main/nextjs-page-router/README.md)

- [Next.js App Router](https://github.com/1zumisawashun/folder-structure-template/blob/main/nextjs-app-router/README.md)


## assets

```
components/
├─ images/ # アプリケーション全体で使う画像を置く
│   └─ image_sample.jpg
└─ styles/ # アプリケーション全体で使うレイアウトを置く（ITCSS）
    ├─ components/
    ├─ elements/
    ├─ generics/
    ├─ objects/
    ├─ settings/
    ├─ tools/
    ├─ utilities/
    └─ app.css
```

ITCSS のガイドラインは[こちら](https://github.com/1zumisawashun/sass-template)

## components

```
components/
├─ buttons/ # アプリケーション全体で使う共通コンポーネントを置く（button系）
│  └─ Button
│     ├─ index.spec.tsx
│     ├─ index.stories.tsx
│     ├─ index.tsx
│     └─ styles.module.css
├─ elements/ # アプリケーション全体で使う共通コンポーネントを置く（その他）
│  └─ Modal
│     ├─ index.spec.tsx
│     ├─ index.stories.tsx
│     ├─ index.tsx
│     └─ styles.module.css
├─ forms/ # アプリケーション全体で使う共通コンポーネントを置く（form系）
│  ├─ InputText/
│  ├─ InputSelect/
│  ├─ InputCheckbox/
│  └─ ...
└─ layouts/ # アプリケーション全体で使うレイアウトコンポーネントを置く（依存先はpagesのみ）
      └─ Header
         ├─ index.spec.tsx
         ├─ index.stories.tsx
         ├─ index.tsx
         └─ styles.module.css
```

## functions

```
functions/
├─ constants/ # アプリケーション全体で使う定数を置く
├─ libs/ # アプリケーション全体で使うライブラリのラッパー関数を置く
├─ helpers/ # アプリケーション全体で使う純粋関数を置く
│     └─ cleanInput/
│        ├─ index.spec.ts
│        └─ index.ts
├─ hooks/ # アプリケーション全体で使うhooksを置く
│     └─ useDisclosure/
│        ├─ index.spec.ts
│        └─ index.ts
├─ models/ # アプリケーション全体で使うエンティティを置く
├─ types/ # アプリケーション全体で使う型定義を置く
```

## features

```
features/
├─ todo/
│  ├─ TodoCreate/ # やること作成
│  ├─ TodoEdit/ # やること編集
│  ├─ TodoDetail/ # やること詳細
│  ├─ TodoList/ # やること一覧
│  │  ├─ components/ # presentation（複数入ることを想定）
│  │  ├─ hooks/ # logic（複数入ることを想定）
│  │  └─ index.tsx # container（最終的にエクスポートする）
│  │
│  ├─ todo.schema.ts # 必要であれば追加
│  └─ todo.type.ts # 必要であれば追加
│
└─...
```

## Troubleshoot

<details>
<summary>ドメインがあるコンポーネントを別のドメインで使いたい</summary>

- 具体的にはプロダクトのドメインがあるProductCardコンポーネントをマイページのドメインでも使用したい場合
- 以下添付画像のようにfeatures/product/components/ProductCardに格納する（components/elementsには格納しない）
- features/ドメイン/〇〇のようにドメイン直下は別ドメインor同ドメインかつ別ページでも使用される
- 例えばProductFormはProductCreateとProductEditで使われているのでfeatures/product/components/に格納している
- hooksや型定義も同様にドメイン直下に配置して暗黙的に使いまわされることを明示する

<img width="374" alt="image" src="https://github.com/1zumisawashun/unifree-client/assets/65071534/c9034c1e-e6b8-459e-ac70-1ce6874d5e78">

</details>


