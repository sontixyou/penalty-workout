# Penalty Workout 仕様書

## 1. プロジェクト概要

### 1.1 プロジェクト名
penalty-workout

### 1.2 目的
FPSゲームで試合に負けた際に、自重トレーニングを行うことで、ゲームの敗北を身体的な成長の機会に変換するWebアプリケーション。

### 1.3 コンセプト
- ゲームの負けをポジティブな行動（筋トレ）に転換
- 自宅で手軽にできる自重トレーニング
- シンプルで使いやすいインターフェース

## 2. 機能仕様

### 2.1 主要機能
1. **手動トリガー機能**
   - 「負けた」ボタンをクリックしてトレーニング開始
   - ワンクリックでアクセス可能

2. **ランダムトレーニング生成**
   - 自重トレーニングメニューをランダムに生成
   - 複数の種目から組み合わせて提示

3. **トレーニング実行画面**
   - 生成されたメニューを表示
   - 各種目の回数/時間を明確に表示
   - 完了ボタンで終了

### 2.2 トレーニング種目
以下の自重トレーニングからランダムに3-5種目を選択：

#### 上半身
- 腕立て伏せ（10-30回）
- ダイヤモンドプッシュアップ（5-15回）
- ワイドプッシュアップ（10-25回）
- パイクプッシュアップ（5-15回）

#### 下半身
- スクワット（15-40回）
- ジャンピングスクワット（10-20回）
- ランジ（左右各10-20回）
- カーフレイズ（20-40回）
- ブルガリアンスクワット（左右各8-15回）

#### 体幹
- プランク（30-60秒）
- サイドプランク（左右各20-40秒）
- バイシクルクランチ（20-40回）
- レッグレイズ（10-25回）
- マウンテンクライマー（20-40回）

#### 全身
- バーピー（5-15回）
- ジャンピングジャック（30-60回）
- ハイニー（30-60秒）

### 2.3 ランダム生成ロジック
```typescript
interface Exercise {
  name: string;
  type: 'upper' | 'lower' | 'core' | 'fullbody';
  unit: 'reps' | 'seconds' | 'reps_each_side';
  minCount: number;
  maxCount: number;
}

interface WorkoutSession {
  exercises: GeneratedExercise[];
  totalTime: number; // 推定所要時間（分）
}

interface GeneratedExercise {
  exercise: Exercise;
  count: number;
}
```

生成ルール：
1. 各カテゴリから最低1種目を選択
2. 合計3-5種目
3. 同じ種目は重複しない
4. 難易度に応じて回数を調整（将来的な拡張）

## 3. 技術仕様

### 3.1 技術スタック
- **フレームワーク**: React 18.x
- **言語**: TypeScript 5.x
- **ビルドツール**: Vite 5.x
- **UIライブラリ**: shadcn/ui
- **スタイリング**: Tailwind CSS
- **テストフレームワーク**: Vitest
- **リンター/フォーマッター**: ESLint, Prettier

### 3.2 プロジェクト構造
```
penalty-workout/
├── src/
│   ├── components/
│   │   ├── ui/              # shadcn/uiコンポーネント
│   │   ├── Button.tsx
│   │   ├── WorkoutDisplay.tsx
│   │   └── ExerciseCard.tsx
│   ├── lib/
│   │   ├── exercises.ts     # トレーニング種目定義
│   │   └── workout-generator.ts
│   ├── types/
│   │   └── index.ts         # TypeScript型定義
│   ├── App.tsx
│   └── main.tsx
├── tests/
│   ├── unit/
│   └── integration/
├── public/
├── docs/
│   └── specification.md
├── package.json
├── tsconfig.json
├── vite.config.ts
├── vitest.config.ts
└── README.md
```

### 3.3 コンポーネント設計

#### App.tsx
- アプリケーションのルートコンポーネント
- 状態管理（現在の画面、生成されたワークアウト）

#### Button.tsx
- 「負けた」ボタン
- shadcn/uiのButtonコンポーネントを使用

#### WorkoutDisplay.tsx
- 生成されたトレーニングメニューの表示
- 完了ボタンの配置

#### ExerciseCard.tsx
- 個別のエクササイズ表示
- 種目名、回数/時間、説明

## 4. UI/UX設計

### 4.1 画面遷移
1. **ホーム画面**
   - 大きな「負けた」ボタンが中央に配置
   - シンプルでクリーンなデザイン

2. **トレーニング画面**
   - 生成されたメニューをカード形式で表示
   - 各種目の詳細が見やすく配置
   - 「完了」ボタンでホームに戻る

### 4.2 デザイン原則
- ダークモード対応（shadcn/uiのテーマ機能）
- レスポンシブデザイン（モバイル優先）
- 高コントラストで視認性重視
- アニメーションは最小限

### 4.3 カラーパレット
- shadcn/uiのデフォルトテーマを使用
- アクセントカラーは赤系（敗北→情熱的なトレーニング）

## 5. 開発環境セットアップ

### 5.1 必要な環境
- Node.js 18.x以上
- npm または yarn

### 5.2 セットアップ手順
```bash
# プロジェクトの作成
npm create vite@latest penalty-workout -- --template react-ts

# 依存関係のインストール
cd penalty-workout
npm install

# shadcn/uiのセットアップ
npx shadcn-ui@latest init

# Vitestのインストール
npm install -D vitest @testing-library/react @testing-library/jest-dom
```

### 5.3 開発コマンド
```bash
# 開発サーバー起動
npm run dev

# ビルド
npm run build

# テスト実行
npm run test

# テスト（ウォッチモード）
npm run test:watch
```

## 6. テスト戦略

### 6.1 ユニットテスト
- ワークアウト生成ロジックのテスト
- 各種ユーティリティ関数のテスト

### 6.2 コンポーネントテスト
- 各コンポーネントの表示テスト
- ユーザーインタラクションのテスト

### 6.3 統合テスト
- 画面遷移のテスト
- 全体的なワークフローのテスト

## 7. デプロイ

### 7.1 推奨デプロイ先
- Vercel
- Netlify
- GitHub Pages

### 7.2 ビルド設定
```json
{
  "build": "vite build",
  "preview": "vite preview"
}
```

## 8. 将来の拡張可能性

### 8.1 機能拡張案
1. **難易度設定**
   - 初心者/中級者/上級者モード
   - カスタム回数設定

2. **音声ガイド**
   - カウントダウン機能
   - 音声での指示

3. **トレーニング動画/アニメーション**
   - 正しいフォームの説明
   - 動画での実演

4. **ゲーム連携**（長期的）
   - API経由での自動検知
   - 複数ゲーム対応

### 8.2 技術的拡張
1. **PWA化**
   - オフライン対応
   - プッシュ通知

2. **多言語対応**
   - i18n実装
   - 英語/日本語切り替え

## 9. セキュリティ・プライバシー

### 9.1 データ保護
- ローカルストレージのみ使用（将来的な設定保存用）
- 個人情報は一切収集しない
- サーバーとの通信なし

### 9.2 ライセンス
- MITライセンスを想定

---

最終更新日: 2025-07-23
