## 全体のディレクトリ構成図

```
src/
├─ @types/
├─ assets/
├─ components/ # 機能に依存しないComponent
├─ features/ # 機能に依存するComponentやHookなど
├─ functions/ # 機能に依存しない関数
├─ pages/ # routingに責務を持つ・実態はfeaturesに配置
├─ providers/
├─ routers/
├─ app.tsx
```

## components ディレクトリ

```
components/
├─ buttons/ # アプリケーション全体で使う共通コンポーネントを置く（button系）
│  └─ Button
│     ├─ index.stories.tsx
│     ├─ styles.module.css
│     └─ index.tsx
├─ elements/ # アプリケーション全体で使う共通コンポーネントを置く（その他）
│  └─ Modal
│     ├─ index.stories.tsx
│     ├─ styles.module.css
│     └─ index.tsx
├─ forms/ # アプリケーション全体で使う共通コンポーネントを置く（form系）
│  ├─ InputText/
│  ├─ InputSelect/
│  ├─ InputCheckbox/
│  └─ ...
└─ layouts/ # アプリケーション全体で使うレイアウトコンポーネントを置く（依存先はpagesのみ）
      └─ Header
         ├─ index.stories.tsx
         ├─ styles.module.css
         └─ index.tsx
```

## functions ディレクトリ

```
functions/
├─ constants/ # アプリケーション全体で使う定数を置く
├─ libs/ # アプリケーション全体で使うライブラリのラッパー関数を置く
├─ helpers/ # アプリケーション全体で使う純粋関数を置く
│     └─ cleanInput/
│        ├─ index.test.ts
│        └─ index.ts
├─ hooks/ # アプリケーション全体で使うhooksを置く
│     └─ useDisclosure/
│        ├─ index.test.ts
│        └─ index.ts
├─ themes/ # アプリケーション全体で使うスタイルを置く
├─ types/ # アプリケーション全体で使う型定義を置く
```

## features ディレクトリ

```
features/
├─ Todo/
│  ├─ TodoCreate/ # やること作成
│  ├─ TodoEdit/ # やること編集
│  ├─ Todo/ # やること詳細
│  └─ Todos/ # やること一覧
│     ├─ components/ # presentation（複数入ることを想定）
│     ├─ hooks/ # logic（複数入ることを想定）
│     ├─ styles/ # style（複数入ることを想定）
│     ├─ tests/ # test（複数入ることを想定）
│     ├─ todos.schema.ts # 必要であれば追加
│     ├─ todos.type.ts
│     └─ index.tsx # container（最終的にエクスポートする）
└─...
```
