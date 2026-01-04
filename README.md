# folder-structure-template

## Overview

React・Next.js のディレクトリ構成の骨子をまとめたリポジトリです

### 前提

- **ルーティング**: Next.js App Router
- **スタイリング**: CSS Modules
- **テスト**: Jest (or Vitest) + React Testing Library
- **UI ドキュメント**: Storybook

```
app/
├─ (pages)/
├─ @types/
├─ assets/
├─ components/
├─ features/
├─ functions/
├─ pages/
├─ providers/
└─ ...
```

## (pages)

- Next.js App Router のルーティングに責務を持つディレクトリ
- `layout.tsx`, `page.tsx`, `loading.tsx`, `error.tsx` などの Next.js の特殊ファイルを配置
- App Router の世界観に従い、ルーティングとデータフェッチングを担当

```
(pages)/
├─ (root)/
│  ├─ layout.tsx
│  └─ page.tsx
├─ about/
│  └─ page.tsx
├─ dashboard/
│  ├─ layout.tsx
│  ├─ page.tsx
│  └─ loading.tsx
└─ ...
```

## @types

- グローバルな型定義を配置するディレクトリ
- 外部ライブラリの型拡張や、アプリケーション全体で使用する共通の型定義を管理

```
@types/
├─ global.d.ts
├─ env.d.ts
└─ ...
```

## assets

- 静的アセット(画像、フォント、スタイル)を管理するディレクトリ
- スタイルは**ITCSS**のアーキテクチャに従って構成

```
assets/
├─ images/
│   ├─ icons/
│   ├─ logos/
│   └─ ...
├─ fonts/
│   └─ ...
└─ styles/
    ├─ settings/      # 変数、カラー、フォントサイズなどの設定
    ├─ tools/         # mixinや関数
    ├─ generics/      # リセットCSS、normalize.css
    ├─ elements/      # 素のHTML要素(h1, a, button等)
    ├─ objects/       # レイアウト用の汎用クラス
    ├─ components/    # 特定のUIコンポーネント用スタイル
    ├─ utilities/     # ユーティリティクラス(margin, padding等)
    └─ app.css        # 上記をインポートするエントリーポイント
```

**参考**: ITCSS の詳細なガイドラインは[こちら](https://github.com/1zumisawashun/sass-template)

## components

- アプリケーション全体で再利用可能な UI コンポーネントを管理
- **Base UI**(headless UI)を推奨 - ロジックと見た目を分離し、柔軟なカスタマイズが可能
- 各コンポーネントには`.module.css`, `.stories.tsx`, `.test.tsx`を含める

```
components/
├─ buttons/
│  └─ Button/
│     ├─ index.module.css
│     ├─ index.stories.tsx
│     ├─ index.test.tsx
│     └─ index.tsx
├─ elements/
│  ├─ Dialog/
│  │  ├─ index.module.css
│  │  ├─ index.stories.tsx
│  │  ├─ index.test.tsx
│  │  └─ index.tsx
│  ├─ Popover/
│  └─ ...
├─ forms/
│  ├─ Checkbox/
│  │  ├─ index.module.css
│  │  ├─ index.stories.tsx
│  │  ├─ index.test.tsx
│  │  └─ index.tsx
│  ├─ TextInput/
│  ├─ Select/
│  └─ ...
└─ layouts/
   └─ VStack/
      ├─ index.module.css
      ├─ index.stories.tsx
      ├─ index.test.tsx
      └─ index.tsx
```

### Base UI (Headless UI) について

- **メリット**:
  - ロジック(状態管理、アクセシビリティ)と見た目を完全に分離
  - プロジェクト固有のデザインシステムに柔軟に対応
  - スタイリングライブラリに依存しない
  - アクセシビリティがデフォルトで担保される
- **推奨ライブラリ**: [Base UI](https://base-ui.com/), [Radix UI](https://www.radix-ui.com/), [Headless UI](https://headlessui.com/)

## functions

- アプリケーション全体で使用する汎用的なロジック・ユーティリティを管理
- ドメインに依存しない、純粋関数や共通処理を配置

```
functions/
├─ constants/         # 定数定義
│  ├─ routes.ts
│  ├─ config.ts
│  └─ ...
├─ helpers/           # ヘルパー関数・ユーティリティ
│  ├─ format/
│  │  ├─ date.ts
│  │  └─ currency.ts
│  ├─ validation/
│  └─ ...
├─ hooks/             # カスタムフック
│  ├─ useDisclosure/
│  │  ├─ index.test.ts
│  │  └─ index.ts
│  ├─ useLocalStorage/
│  ├─ useDebounce/
│  └─ ...
├─ libs/              # 外部ライブラリのラッパー・初期化
│  ├─ axios.ts
│  ├─ dayjs.ts
│  └─ ...
└─ types/             # 共通型定義
   ├─ api.ts
   ├─ models.ts
   └─ ...
```

### 配置の指針

- **features vs functions**:
  - 特定のドメイン(todo, user 等)に紐づく → `features/`
  - 汎用的でドメインに依存しない → `functions/`
- **hooks**: ビジネスロジックを含まない汎用的な hooks のみ配置
- **types**: API レスポンス型、共通モデル型など、複数箇所で使用される型

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

## providers

- アプリケーション全体で使用する Context や Provider、状態管理を配置
- React Context API、TanStack Query、Jotai、Zustand などのプロバイダーを管理

```
providers/
├─ QueryProvider.tsx
├─ ThemeProvider.tsx
├─ AuthProvider.tsx
└─ ...
```
