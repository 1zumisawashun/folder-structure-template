# folder-structure-template

- React・Next.js のディレクトリ構成の骨子をまとめたリポジトリです

## Overview

- [React](https://github.com/1zumisawashun/folder-structure-template/blob/main/react-vite/README.md)

- [Next.js Page Router](https://github.com/1zumisawashun/folder-structure-template/blob/main/nextjs-page-router/README.md)

- [Next.js App Router](https://github.com/1zumisawashun/folder-structure-template/blob/main/nextjs-app-router/README.md)

## assets

```
assets/
├─ images/
│   └─ image_sample.jpg
└─ styles/ # ITCSSを採用している
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
├─ buttons/
│  └─ Button
│     ├─ index.spec.tsx
│     ├─ index.stories.tsx
│     ├─ index.tsx
│     └─ styles.module.css
├─ elements/
│  └─ Dialog
│     ├─ index.spec.tsx
│     ├─ index.stories.tsx
│     ├─ index.tsx
│     └─ styles.module.css
├─ forms/
│  ├─ InputText/
│  ├─ InputSelect/
│  ├─ InputCheckbox/
│  └─ ...
└─ layouts/ # pagesディレクトリに依存する
      └─ Header
         ├─ index.spec.tsx
         ├─ index.stories.tsx
         ├─ index.tsx
         └─ styles.module.css
```

## functions

```
functions/
├─ constants/
├─ helpers/
├─ hooks/
│     └─ useDisclosure/
│        ├─ index.spec.ts
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

<img width="374" alt="image" src="https://github.com/1zumisawashun/unifree-client/assets/65071534/c9034c1e-e6b8-459e-ac70-1ce6874d5e78">

</details>
