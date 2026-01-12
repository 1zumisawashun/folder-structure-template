# folder-structure-template

## 目次

- [Overview](#overview)
  - [技術選定](#技術選定)
  - [全体のディレクトリ構成](#全体のディレクトリ構成)
- [(pages)](#pages)
- [@types](#types)
- [assets](#assets)
- [components](#components)
- [functions](#functions)
- [features](#features)
- [pages](#pages-1)
- [providers](#providers)
- [トラブルシューティング](#トラブルシューティング)

## Overview

React・Next.js のディレクトリ構成の骨子をまとめたリポジトリです

### 技術選定

- **ルーティング**: Next.js App Router
- **スタイリング**: CSS Modules
- **テスト**: Jest (or Vitest) + React Testing Library
- **UI ドキュメント**: Storybook

### 全体のディレクトリ構成

```
app/
├─ (pages)/       # Next.js App Routerのルーティング
├─ @types/        # グローバルな型定義
├─ assets/        # 静的アセット(画像、フォント、スタイル)
├─ components/    # 全ドメインで汎用的に使用するUIコンポーネント
├─ features/      # ドメインを跨いで使用されるコンポーネントと型定義
├─ functions/     # 汎用的なロジック・ユーティリティ
├─ pages/         # 実装の本丸 - 各ページの実装
├─ providers/     # Context、Provider、状態管理
└─ ...
```

## (pages)

- **データフェッチとルーティングに責務を持つ**ディレクトリ
- `layout.tsx`, `page.tsx`, `loading.tsx`, `error.tsx` などの Next.js の特殊ファイルを配置する
- **実装の詳細**: 各`page.tsx`は`pages/`ディレクトリ内のコンポーネントをインポートして使用する
- **データフェッチング**: 基本的にここでデータフェッチを行い、データソースから取得したデータをそのまま`pages/`に渡す
- **インターフェース**: データソース(API レスポンス)に依存する形で`pages/`のインターフェースを定義する
- **変換処理**: データの変換(transform)は行わず、取得したデータをそのまま`pages/`に渡して表示ロジックを委譲

### ディレクトリ構成

```
(pages)/
├─ (root)/
│  ├─ layout.tsx
│  └─ page.tsx         # pages/xxx をインポート
├─ about/
│  └─ page.tsx
├─ dashboard/
│  ├─ layout.tsx
│  ├─ page.tsx
│  └─ loading.tsx
└─ ...
```

### インポート例

```tsx
// (pages)/articles/create/page.tsx
import { ArticleCreate } from "@/pages/articles/create/ArticleCreate";

export default function Page() {
  return <ArticleCreate />;
}
```

## @types

- グローバルな型定義を配置するディレクトリ
- 外部ライブラリの型拡張や、アプリケーション全体で使用する共通の型定義を管理

### ディレクトリ構成

```
@types/
├─ global.d.ts
├─ env.d.ts
└─ ...
```

## assets

- 静的アセット(画像、フォント、スタイル)を管理するディレクトリ
- スタイルは**ITCSS**のアーキテクチャに従って構成

### ディレクトリ構成

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

### ディレクトリ構成

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

### ディレクトリ構成

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

## features

- ドメイン(機能単位)ごとに、**ドメインを跨いで使用されるコンポーネント**や型定義を管理するディレクトリ
- 同一ドメイン内のみで使うコンポーネントは`pages/[domain]/shared/`に配置

### ディレクトリ構成

```
features/
├─ articles/                    # articlesドメイン
│  ├─ articleCard/              # ドメインを跨いで使うコンポーネント
│  │  ├─ ArticleCard.tsx       # 例: mypageでも使用
│  │  └─ ArticleCard.stories.tsx
│  ├─ articleCardGroup/
│  │  ├─ ArticleCardGroup.tsx
│  │  └─ ArticleCardGroup.stories.tsx
│  │
│  ├─ articles.schema.ts        # zodスキーマ定義
│  └─ articles.type.ts          # 型定義
│
└─ mypage/
   └─ ...
```

**注意事項**:

- **datasourceのレスポンス型を直接インターフェースに定義することは避ける**
  - 理由: 複数のドメインで同じコンポーネントを使う場合、datasourceに依存する型(Branded-Type等)をユニオン型で扱う必要が出てくるため
  - 例: `ArticleCard`をarticlesドメインとmypageドメインの両方で使う場合、各ドメインのdatasource型が異なると型の整合性が取れなくなる
  - 推奨: propsとして必要な最小限のプリミティブな型を定義し、datasourceの型に直接依存させない

## pages

- **実装の本丸**: 基本的にはここにコードを追加していく
- `(pages)/`と同じディレクトリ構成で、各ページの実装を配置
- ページ専用のコンポーネントは`components/`配下に配置

### ディレクトリ構成

```
pages/
├─ articles/
│  ├─ (list)/
│  │  └─ ArticleList.tsx
│  ├─ [id]/
│  │  └─ ArticleDetail.tsx
│  ├─ create/
│  │  ├─ components/              # このページ専用のコンポーネント
│  │  │  └─ ArticleCreateForm.tsx
│  │  ├─ ArticleCreate.tsx
│  │  └─ ArticleCreate.stories.tsx
│  └─ shared/                      # 同じドメイン内で共通のコンポーネント
│     └─ ArticleForm.tsx          # 例: 追加と編集で共通のフォーム
├─ mypage/
│  └─ ...
└─ ...
```

### pages 配下のディレクトリ

#### `components/`

- **配置対象**: そのページ専用のコンポーネント
- **スコープ**: 単一ページ内でのみ使用
- **例**: `pages/articles/create/components/ArticleCreateForm.tsx`

#### `shared/`

- **配置対象**: 同じドメイン内で複数ページで共通利用するコンポーネント
- **スコープ**: 同一ドメイン内の複数ページで使用
- **例**: `pages/articles/shared/ArticleForm.tsx` - 追加と編集で共通のフォーム
- **使用例**: `ArticleCreateForm`が`ArticleForm`を使って作成ページ用のフォームを構築

## providers

- アプリケーション全体で使用する Context や Provider、状態管理を配置
- React Context API、TanStack Query、Jotai、Zustand などのプロバイダーを管理

### ディレクトリ構成

```
providers/
├─ QueryProvider.tsx
├─ ThemeProvider.tsx
├─ AuthProvider.tsx
└─ ...
```

## トラブルシューティング

### コンポーネントの配置

**概要**: 使用箇所に応じた配置先の判断基準

| 使用箇所                   | 配置先                              | 具体例                           |
| -------------------------- | ----------------------------------- | -------------------------------- |
| 単一ページ内のみ           | `pages/[domain]/[page]/components/` | 記事作成ページ専用のエディタ     |
| 同一ドメイン内の複数ページ | `pages/[domain]/shared/`            | 記事の追加・編集で共通のフォーム |
| 複数ドメインで使用         | `features/[domain]/`                | マイページで表示する記事カード   |
| 全ドメインで汎用的に使用   | `components/`                       | Button, Dialog 等の基本 UI       |

**詳細**: 各配置先の説明

#### 1. `pages/[domain]/[page]/components/`

- **配置対象**: そのページ専用のコンポーネント
- **スコープ**: 単一ページ内でのみ使用
- **例**: `pages/articles/create/components/ArticleCreateForm.tsx`

#### 2. `pages/[domain]/shared/`

- **配置対象**: 同じドメイン内で複数ページで共通利用するコンポーネント
- **スコープ**: 同一ドメイン内の複数ページで使用
- **例**: `pages/articles/shared/ArticleForm.tsx` - 追加と編集で共通のフォーム

#### 3. `features/[domain]/` (例: `articleCard/`)

- **配置対象**: ドメインを跨いで使用されるコンポーネント
- **スコープ**: 他のドメインからも参照可能
- **例**: `features/articles/articleCard/` - mypage ドメインからも使用

#### 4. `components/`

- **配置対象**: 全ドメインで汎用的に使用する基本 UI コンポーネント
- **スコープ**: アプリケーション全体
- **例**: `components/buttons/Button/`, `components/elements/Dialog/`

### 関数・ユーティリティの配置

- ドメインに依存しない汎用的な関数・hooks → `functions/`

### 型定義の配置

- ドメイン固有の型 → `features/[domain]/`
- 汎用的な共通型 → `functions/types/`
- グローバル型定義 → `@types/`

### datasourceの依存関係

**概要**: データフェッチ(datasource)に対する各ディレクトリの依存関係ルール

| ディレクトリ                        | datasource依存 | 説明                                                      |
| ----------------------------------- | -------------- | --------------------------------------------------------- |
| `(pages)/`                          | ✅ 依存する     | データフェッチロジックを実装し、`pages/`にデータを渡す    |
| `pages/[domain]/[page]/`            | ✅ 依存する     | `(pages)/`から渡されたデータを表示する                    |
| `pages/[domain]/shared/`            | ❌ 依存しない   | propsで受け取ったデータを表示する純粋なUIコンポーネント   |
| `pages/[domain]/[page]/components/` | ❌ 依存しない   | propsで受け取ったデータを表示する純粋なUIコンポーネント   |
| `features/[domain]/`                | ❌ 依存しない   | propsで受け取ったデータを表示する純粋なUIコンポーネント   |
| `components/`                       | ❌ 依存しない   | 全ドメインで再利用可能な純粋なUIコンポーネント            |

**詳細**: 各ディレクトリの役割

#### `(pages)/` - データフェッチ層

- **役割**: データソース(API、データベース等)からデータを取得
- **責務**: データフェッチロジックの実装
- **データの流れ**: datasource → `(pages)/` → `pages/`
- **実装例**:
  ```tsx
  // (pages)/articles/create/page.tsx
  import { ArticleCreate } from "@/pages/articles/create/ArticleCreate";
  import { fetchCategories } from "@/datasources/categories";

  export default async function Page() {
    const categories = await fetchCategories();
    return <ArticleCreate categories={categories} />;
  }
  ```

#### `pages/[domain]/[page]/` - プレゼンテーション層(データ依存)

- **役割**: `(pages)/`から渡されたデータを使って画面を構築
- **責務**: 表示ロジックの実装
- **データの流れ**: `(pages)/`から受け取ったデータを`shared/`や`components/`に渡す
- **実装例**:
  ```tsx
  // pages/articles/create/ArticleCreate.tsx
  export function ArticleCreate({ categories }: { categories: Category[] }) {
    return <ArticleForm categories={categories} />;
  }
  ```

#### `pages/[domain]/shared/` - プレゼンテーション層(純粋UI)

- **役割**: 同一ドメイン内で再利用可能なUIコンポーネント
- **責務**: propsで受け取ったデータの表示のみ
- **データの流れ**: propsとして受け取り、表示する
- **実装例**:
  ```tsx
  // pages/articles/shared/ArticleForm.tsx
  export function ArticleForm({ categories }: { categories: Category[] }) {
    return (
      <form>
        <CategorySelect categories={categories} />
      </form>
    );
  }
  ```

**注意事項**: 

- **datasourceのレスポンス型を直接インターフェースに定義することは避ける**
  - 理由: 追加・編集など複数の画面で同じコンポーネントを使う場合、datasourceに依存する型(Branded-Type等)をユニオン型で扱う必要が出てくるため
  - 例: `ArticleForm`を作成と編集の両方で使う場合、`Article | ArticleDraft`のようなユニオン型が必要になり、型の複雑性が増す
  - 推奨: propsとして必要な最小限のプリミティブな型を定義し、datasourceの型に直接依存させない

**ベストプラクティス**:

1. **データフェッチは`(pages)/`でのみ実行**: 他の層ではデータフェッチを行わない
2. **`shared/`と`components/`は純粋に保つ**: datasourceに依存せず、テストが容易で再利用性が高い状態を維持
3. **データ変換は最小限に**: `(pages)/`ではデータ変換を行わず、取得したデータをそのまま渡す
4. **型安全性の確保**: datasourceのレスポンス型を`pages/`のprops型として定義し、型安全性を保つ
