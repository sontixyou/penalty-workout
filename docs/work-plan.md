# Penalty Workout 作業計画書

## 1. プロジェクト概要

**プロジェクト名**: penalty-workout  
**目的**: FPSゲームの敗北を自重トレーニングの機会に変換するWebアプリケーション  
**作成日**: 2025-07-23

## 2. 作業フェーズと見積もり時間

### フェーズ1: 環境構築とセットアップ（1時間）

#### 1.1 プロジェクト初期化（15分）

- [ ] Vite + React + TypeScriptでプロジェクトを作成

  ```bash
  npm create vite@latest penalty-workout -- --template react-ts
  cd penalty-workout
  npm install
  ```

#### 1.2 必要な依存関係のインストール（30分）

- [ ] shadcn/uiのセットアップ

  ```bash
  npx shadcn-ui@latest init
  ```

- [ ] Tailwind CSSの設定確認
- [ ] Vitestとテスト関連ライブラリのインストール

  ```bash
  npm install -D vitest @testing-library/react @testing-library/jest-dom
  ```

- [ ] ESLint, Prettierの設定

#### 1.3 プロジェクト構造の作成（15分）

- [ ] ディレクトリ構造の作成
  - src/components/
  - src/lib/
  - src/types/
  - tests/unit/
  - tests/integration/

### フェーズ2: データモデルとロジック実装（2時間）

#### 2.1 TypeScript型定義（30分）

- [ ] `src/types/index.ts`の作成
  - Exercise型の定義
  - WorkoutSession型の定義
  - GeneratedExercise型の定義

#### 2.2 エクササイズデータの定義（45分）

- [ ] `src/lib/exercises.ts`の作成
  - 上半身エクササイズ（4種類）
  - 下半身エクササイズ（5種類）
  - 体幹エクササイズ（5種類）
  - 全身エクササイズ（3種類）

#### 2.3 ワークアウト生成ロジック（45分）

- [ ] `src/lib/workout-generator.ts`の作成
  - ランダム選択アルゴリズム
  - カテゴリバランスの考慮
  - 回数/時間のランダム生成

### フェーズ3: UIコンポーネント実装（3時間）

#### 3.1 shadcn/ui基本コンポーネント導入（30分）

- [ ] Button コンポーネントの追加
- [ ] Card コンポーネントの追加
- [ ] その他必要なUIコンポーネント

#### 3.2 カスタムコンポーネント実装（2時間）

- [ ] `src/components/Button.tsx`
  - 「負けた」ボタンのカスタマイズ
  - アニメーション効果
- [ ] `src/components/ExerciseCard.tsx`
  - エクササイズ情報の表示
  - 回数/時間の見やすい表示
- [ ] `src/components/WorkoutDisplay.tsx`
  - ワークアウトセッション全体の表示
  - 完了ボタンの配置

#### 3.3 レスポンシブデザイン対応（30分）

- [ ] モバイル表示の最適化
- [ ] タブレット・デスクトップ対応

### フェーズ4: アプリケーションロジック実装（2時間）

#### 4.1 App.tsx実装（1時間）

- [ ] 状態管理の実装
  - 現在の画面状態（home/workout）
  - 生成されたワークアウトデータ
- [ ] 画面遷移ロジック
- [ ] イベントハンドラー

#### 4.2 メインエントリーポイント（30分）

- [ ] `src/main.tsx`の設定
- [ ] グローバルスタイルの適用

#### 4.3 スタイリングとテーマ（30分）

- [ ] ダークモード対応
- [ ] カラーパレットの適用（赤系アクセント）
- [ ] アニメーションの実装

### フェーズ5: テストとビルド設定（1.5時間）

#### 5.1 ユニットテスト作成（45分）

- [ ] ワークアウト生成ロジックのテスト
- [ ] ユーティリティ関数のテスト

#### 5.2 コンポーネントテスト（30分）

- [ ] 各コンポーネントの表示テスト
- [ ] ユーザーインタラクションテスト

#### 5.3 ビルド設定（15分）

- [ ] vite.config.tsの最適化
- [ ] package.jsonスクリプトの設定

### フェーズ6: 最終調整とドキュメント（30分）

#### 6.1 READMEの作成

- [ ] プロジェクト概要
- [ ] セットアップ手順
- [ ] 使用方法

#### 6.2 最終動作確認

- [ ] 全体的な動作テスト
- [ ] ビルド結果の確認

## 3. 技術的な実装詳細

### 3.1 ワークアウト生成アルゴリズム

```typescript
// 疑似コード
function generateWorkout(): WorkoutSession {
  const selectedExercises: GeneratedExercise[] = []
  const categories = ['upper', 'lower', 'core', 'fullbody']
  
  // 各カテゴリから最低1種目選択
  categories.forEach(category => {
    const exercise = selectRandomFromCategory(category)
    selectedExercises.push(generateExerciseWithCount(exercise))
  })
  
  // 残りをランダムに選択（合計3-5種目）
  while (selectedExercises.length < randomBetween(3, 5)) {
    // 重複チェックしながら追加
  }
  
  return {
    exercises: selectedExercises,
    totalTime: calculateTotalTime(selectedExercises)
  }
}
```

### 3.2 UI/UX設計ポイント

1. **ホーム画面**
   - 中央に大きな「負けた」ボタン
   - シンプルで直感的なデザイン
   - クリック時のフィードバック

2. **ワークアウト画面**
   - カード形式でエクササイズを表示
   - 進捗が分かりやすい表示
   - 完了時の達成感を演出

### 3.3 パフォーマンス最適化

- React.memoを使用したコンポーネント最適化
- 画像アセットの遅延読み込み（将来的な拡張用）
- ビルド時のコード分割

## 4. リスクと対策

### 4.1 技術的リスク

- **リスク**: shadcn/uiの初期設定でエラーが発生する可能性
- **対策**: 公式ドキュメントに従い、段階的に導入

### 4.2 スケジュールリスク

- **リスク**: UIコンポーネントの実装に予想以上の時間がかかる
- **対策**: 基本機能を優先し、装飾的な要素は後回しにする

## 5. 成果物

1. 動作するWebアプリケーション
2. テストコード
3. ドキュメント（README、仕様書）
4. ビルド済みの配布可能ファイル

## 6. 将来の拡張に向けた準備

- コードの拡張性を考慮した設計
- 新しいエクササイズを簡単に追加できる構造
- 難易度設定機能の追加を見据えたデータ構造

---

**総見積もり時間**: 約10時間

**優先順位**:

1. 基本機能の実装（MVP）
2. UI/UXの改善
3. テストとドキュメント

この計画に従って段階的に実装を進めることで、確実に動作するアプリケーションを構築できます。

