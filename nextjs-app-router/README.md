## 全体のディレクトリ構成図

```
app/
├─ (pages)/ # routingにのみ関心を持つ・実態はfeaturesに配置する
├─ @types/
├─ assets/
├─ components/ # layer
├─ features/ # feature
├─ functions/ # layer
├─ providers/
```

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

> 非同期コンポーネントにすることでStreaming SSR + Suspenseができる

### トラブルシューティング

- 実装上どうしても(pages)ディレクトリ以外でフェッチをしなくてはいけない時は？
  - Presentation / Container パターンを使う
