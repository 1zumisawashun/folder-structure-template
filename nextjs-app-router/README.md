## 全体のディレクトリ構成図

```
app/
├─ (pages)/ # routingにのみ関心を持つ・実態はfeaturesに配置する
├─ @types/
├─ assets/
├─ components/ # 機能に依存しないcomponent
├─ features/ # 機能に依存するcomponentやFunctionなど
├─ functions/ # 機能に依存しないfunction
├─ providers/ # 依存先は(pages)/layout.tsx
```
