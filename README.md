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

- ドメイン(機能単位)ごとに共通コンポーネントや型定義を管理するディレクトリ

### ディレクトリ構成

```
features/
├─ articles/                    # articlesドメイン
│  ├─ shared/                   # 同じドメイン内で共通のコンポーネント
│  │  └─ articleForm/          # 例: 編集と追加で共通のフォーム
│  │     ├─ ArticleForm.tsx
│  │     └─ ArticleForm.stories.tsx
│  │
│  ├─ articleCard/              # ドメインを跨いで使うコンポーネント
│  ├─ articleCardGroup/         # 例: mypageでarticleCardを使う場合
│  │
│  ├─ articles.schema.ts        # zodスキーマ定義
│  └─ articles.type.ts          # 型定義
│
└─ mypage/
   └─ ...
```

### 共通コンポーネントの分け方

#### 1. `pages/[page]/components/`

- **配置対象**: そのページ専用のコンポーネント
- **スコープ**: 単一ページ内でのみ使用
- **例**: `pages/articles/create/components/ArticleCreateForm.tsx`

#### 2. `features/[domain]/shared/`

- **配置対象**: 同じドメイン内で複数ページで共通利用するコンポーネント
- **スコープ**: 同一ドメイン内でのみ使用
- **例**: `features/articles/shared/articleForm/` - 追加と編集で共通のフォーム

#### 3. `features/[domain]/` 直下 (例: `articleCard/`)

- **配置対象**: ドメインを跨いで使用されるコンポーネント
- **スコープ**: 他のドメインからも参照可能
- **例**: `features/articles/articleCard/` - mypage ドメインからも使用

### 配置の判断基準

| 使用箇所                   | 配置先                     | 具体例                           |
| -------------------------- | -------------------------- | -------------------------------- |
| 単一ページ内のみ           | `pages/[page]/components/` | 記事作成ページ専用のエディタ     |
| 同一ドメイン内の複数ページ | `shared/`                  | 記事の追加・編集で共通のフォーム |
| 複数ドメインで使用         | ドメイン直下               | マイページで表示する記事カード   |
| 全ドメインで汎用的に使用   | `components/`              | Button, Dialog 等の基本 UI       |

## pages

- **実装の本丸**: 基本的にはここにコードを追加していく
- `(pages)/`と同じディレクトリ構成で、各ページの実装を配置
- ページ専用のコンポーネントは`components/`配下に配置

```
pages/
├─ articles/
│  ├─ (list)/
│  │  └─ ArticleList.tsx
│  ├─ [id]/
│  │  └─ ArticleDetail.tsx
│  └─ create/
│     ├─ components/              # このページ専用のコンポーネント
│     │  └─ ArticleCreateForm.tsx
│     ├─ ArticleCreate.tsx
│     └─ ArticleCreate.stories.tsx
├─ mypage/
│  └─ ...
└─ ...
```

### pages 配下の `components/`

- **配置対象**: そのページ専用のコンポーネント
- **スコープ**: 単一ページ内でのみ使用
- **例**: `pages/articles/create/components/ArticleCreateForm.tsx`

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
