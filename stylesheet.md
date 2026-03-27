# styles.css スタイルシートドキュメント

## 1. プロジェクト全体のスタイル概要

このスタイルシートは、IBM Design Languageに基づいたFAQサイトのための包括的なスタイル定義です。モダンで洗練されたUIを実現するため、IBM Plex フォントファミリーとIBMカラーパレットを採用し、レスポンシブデザインとアクセシビリティに配慮した設計となっています。

### 主な特徴
- IBM Design Languageの厳格な適用
- CSS変数（カスタムプロパティ）による一貫したデザインシステム
- レスポンシブデザイン（モバイル、タブレット、デスクトップ対応）
- スムーズなアニメーションとトランジション
- アクセシビリティへの配慮

---

## 2. 主要なデザイン要素

### 2.1 カラーパレット

#### プライマリカラー
- **IBM Blue**: `--ibm-blue: #0f62fe` - メインのアクセントカラー
- **IBM Blue Dark**: `--ibm-blue-dark: #0043ce` - ホバー状態用
- **IBM Blue Light**: `--ibm-blue-light: #4589ff` - 補助的なアクセント

#### グレースケール
- `--ibm-gray-10: #f4f4f4` - 背景色（最も明るい）
- `--ibm-gray-20: #e0e0e0` - ボーダー、区切り線
- `--ibm-gray-30: #c6c6c6` - 非アクティブ要素
- `--ibm-gray-50: #8d8d8d` - セカンダリテキスト
- `--ibm-gray-70: #525252` - 通常テキスト
- `--ibm-gray-90: #262626` - 見出し
- `--ibm-gray-100: #161616` - ヘッダー、フッター背景（最も暗い）

#### セマンティックカラー
- `--ibm-white: #ffffff` - 白色
- `--ibm-green: #24a148` - 成功状態
- `--ibm-red: #da1e28` - エラー、インラインコード
- `--ibm-yellow: #f1c21b` - 警告状態

### 2.2 スペーシングシステム

一貫したスペーシングのための変数定義：
- `--spacing-xs: 0.25rem` (4px)
- `--spacing-sm: 0.5rem` (8px)
- `--spacing-md: 1rem` (16px)
- `--spacing-lg: 1.5rem` (24px)
- `--spacing-xl: 2rem` (32px)
- `--spacing-2xl: 3rem` (48px)

### 2.3 タイポグラフィ

#### フォントファミリー
- **メインフォント**: `--font-family: 'IBM Plex Sans'` + システムフォントフォールバック
- **等幅フォント**: `--font-family-mono: 'IBM Plex Mono'` + 等幅フォールバック

#### フォントサイズ階層
- `html { font-size: 16px }` - ベースサイズ
- H1: `2.5rem` (40px)
- H2: `2rem` (32px)
- H3: `1.5rem` (24px)
- H4: `1.25rem` (20px)
- 本文: `1rem` (16px)
- 小テキスト: `0.875rem` (14px)

### 2.4 その他のデザイントークン

- **ボーダー半径**: `--border-radius: 0` - IBMデザインの特徴的な直角
- **トランジション**: `--transition: all 0.2s cubic-bezier(0.2, 0, 0.38, 0.9)` - スムーズなアニメーション

---

### 5.8 IBM Design Languageの厳格な適用

- 角丸なし（`border-radius: 0`）
- IBM Plexフォントファミリー
- IBMカラーパレットの使用
- 一貫したスペーシングシステム
