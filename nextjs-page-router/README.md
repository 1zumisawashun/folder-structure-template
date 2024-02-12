## 全体のディレクトリ構成図

```
src/
├─ @types/
├─ assets/
├─ components/ # 機能に依存しないcomponent
├─ features/ # 機能に依存するcomponentやFunctionなど
├─ functions/ # 機能に依存しないfunction
├─ pages/ # routingにのみ関心を持つ・実態はfeaturesに配置する
├─ providers/ # 依存先はpages/app.tsx
```
