# IBM Bobの画面構成と基本操作

![IBM Bobの画面構成](../assets/images/02_bob_interface/interface_overview.png)

IBM Bobの画面構成と基本的な操作方法を理解します。

## 画面構成の理解

### 1. チャット画面の表示
<img src="../assets/images/02_bob_interface/chat_screen_icon.png" alt="Chat Screen Icon">

IBM Bob画面上部の**Bobアイコン**をクリックすると、画面右側にチャット画面が開きます。


### 2. 画面の調整

**チャット画面の幅調整:**

- チャット画面の境界をドラッグして、見やすい幅に調整できます

<img src="../assets/images/02_bob_interface/chat_width_adjust.png" alt="Chat Width Adjustment">

**EXPLORERの表示/非表示:**

- 左上のファイルアイコンをクリックで切り替え

<img src="../assets/images/02_bob_interface/explorer_toggle.png" alt="Explorer Toggle">

**その他のパネル:**

- 画面右上のアイコンで、ファイル表示やターミナルを切り替えできます

<img src="../assets/images/02_bob_interface/panel_icons.png" alt="Panel Icons">


## IBM Bobのモード

### 1. モードの確認
チャット画面左下に、現在使用しているモードが表示されています。
この部分をクリックすると、利用可能なモードの一覧が表示されます。

<img src="../assets/images/02_bob_interface/mode_selection_v2.png" alt="Mode Selection">

### 2. 各モードの説明

v2 では **Agent / Plan / Ask** の 3 モードが用意されています。やりたいことに合わせて切り替えます。

| モード | 役割 | 使う場面 |
|--------|------|---------|
| **Agent** 🤖 | コードを書く・直す | 機能実装、バグ修正、リファクタリング |
| **Plan** 📝 | 進め方を設計する | 実装前に方針や手順を整理したいとき |
| **Ask** ❓ | 質問に答える | コードや技術について知りたいとき（変更なし） |


## 設定の確認


### 1. 設定画面を開く

<img src="../assets/images/02_bob_interface/settings_icon_v2.png" alt="Settings Icon">
チャット画面右上の**歯車アイコン**をクリックすると、設定画面が開きます。

### 2. 主な設定項目

- **バジェット**: 使用済みバジェットの確認
- **言語の選択**: Bobの言語設定（必要に応じて日本語に切り替え）

<img src="../assets/images/02_bob_interface/settings_budget_v2.png" alt="Settings Screen - Budget">

<img src="../assets/images/02_bob_interface/settings_language_v2.png" alt="Settings Screen - Language">

## 自動承認機能

### 1. 自動承認とは

Bobが行うアクション（ファイルの書き込み、コマンドの実行など）を自動的に承認する機能です。

**自動承認オンの場合:**

- 指定した操作は自動承認される
- 作業がスムーズに進む

**自動承認オフの場合:**

- 各操作ごとに確認が必要
- Bobの動作を学習できる

<img src="../assets/images/02_bob_interface/auto_approval_off.png" alt="Auto Approval Off">

### 2. 自動承認の設定確認

1. チャット画面上部の「自動承認（Auto-approval）」スイッチにカーソルを合わせる
2. 自動承認するアクションの一覧が表示されるので確認
3. 確認後、スイッチを元の状態に戻す

**推奨設定:**

- **初心者**: オフ（各ステップで確認しながら学習）
- **慣れてきたら**: オン（効率的に作業）

<img src="../assets/images/02_bob_interface/permission_settings_v2.png" alt="Permission Settings">


## チェックリスト


IBM Bobの画面構成と基本操作を理解したら、以下を確認してください：

- チャット画面の表示方法を理解
- 画面の調整方法を理解
- 各モードの役割を理解
- 設定画面の使い方を理解
- 自動承認機能を理解

## 次のステップ

IBM Bobの基本操作を理解したら、実際のチュートリアルに進みましょう！

---

**前へ**: [準備と設定](01_preparation_and_setup.md) | **次へ**: [LAB1 - 初期開発](03_lab1.md)