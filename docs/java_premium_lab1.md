# Lab 1 — Liberty への移行

## Simple Pharmacy Dashboard：WebSphere から Liberty への移行

---

## 目次

1. [はじめに](#1-はじめに)
2. [前提条件](#2-前提条件)
3. [V2 の注目機能](#3-v2-の注目機能)
4. [Java Modernization ワークフローの概要](#4-java-modernization-ワークフローの概要)
5. [セットアップ](#5-セットアップ)
6. [演習 1：Liberty Replatforming ワークフローを実行する](#6-演習-1liberty-replatforming-ワークフローを実行する)
7. [演習 2：移行後のアプリを確認する](#7-演習-2移行後のアプリを確認する)
8. [トラブルシューティング](#8-トラブルシューティング)
9. [まとめ](#9-まとめ)

---

## 1. はじめに

### 対象アプリケーション

**Simple Pharmacy Management System** は、薬局業務を管理する Web アプリケーションです。

- **処方箋（Prescriptions）**: 患者の処方箋を作成・管理
- **注文（Orders）**: 薬品注文の処理・決済
- **薬品（Medicines）**: 在庫管理
- **ダッシュボード（Dashboard）**: 未処理の処方箋・注文を一覧表示

### Java Modernization とは

Java Modernization は、レガシー Java アプリケーションを最新のバージョンやプラットフォームへ移行するプロセスです。主な作業は以下の通りです。

- **Java バージョンアップグレード**: Java 8 などの古いバージョンから Java 21 などの LTS バージョンへの移行
- **アプリケーションサーバー移行**: Traditional WebSphere などの旧サーバーから Liberty などの軽量ランタイムへの移行
- **依存ライブラリの更新**: 現在サポートされているバージョンへのアップグレード
- **コードの変換**: 最新 Java の機能を活用したコードへの更新

### このラボについて

このラボでは **Bob V2 の Java Modernization ワークフロー** を使い、レガシーな薬局アプリケーションをモダナイズします。ワークフロー内で **Liberty Replatforming** サブタイプを選択し、Traditional WebSphere から Liberty へ移行します。

| 項目 | 内容 |
|------|------|
| **移行前** | Traditional WebSphere（TWas）、Java 8、Struts |
| **移行後** | Liberty ランタイム、Java 8、Struts（Java バージョンアップは Lab 2 で実施） |

### 学習目標

このラボを完了すると、次のことができるようになります。

- Bob V2 の Java Modernization ワークフローをエンドツーエンドで実行できる
- Bob が各移行変更に対して使うインタラクティブな承認フローを理解できる
- Bob のタスクごとのコスト・モダナイゼーションサマリーを読み解ける
- 移行後の Liberty アプリケーションをデプロイして動作確認できる

---

## 2. 前提条件

### IBM Bob IDE（V2）

- 最新の Bob V2 IDE 拡張機能をインストール済みであること
- Java Modernization ワークフローが含まれる Premium パッケージのサブスクリプションが必要
- Bob にログインしていること

詳細は「[準備編](java_premium_00_preparation.md)」を参照してください。

### SDKMAN!（SDK マネージャー）

SDKMAN! は、Unix 系システムで複数の SDK バージョンを管理するツールです。Liberty Replatforming ワークフローは自動でツールをインストールできますが、事前にシェル環境を整えておくとスムーズです。

**インストール手順（macOS / zsh）**

```bash
curl -s "https://get.sdkman.io" | bash
echo '[[ -s "$HOME/.sdkman/bin/sdkman-init.sh" ]] && source "$HOME/.sdkman/bin/sdkman-init.sh"' >> ~/.zshrc
source ~/.zshrc
```

**インストール確認**

```bash
sdk version
# 出力例: SDKMAN! 5.x.x
```

### Java Development Kit（JDK）— 任意

Java 8 はこのラボで必要です。Bob V2 は不足している場合に自動インストールできますが、事前に用意しておくことも可能です。

!!! warning "Apple Silicon（M シリーズ）をお使いの方へ"
    Temurin 8 は Apple Silicon では利用できません。**Zulu ディストリビューション**を使用してください。

**動作確認済み（Apple Silicon）**: `8.0.492-zulu`

```bash
sdk install java 8.0.492-zulu
sdk default java 8.0.492-zulu
java -version  # 1.8.x と表示されることを確認
```

### Maven

```bash
sdk install maven
mvn --version
```

### Bob の再起動

!!! warning "重要な手順"
    Maven を SDKMAN! でインストールした後は、**Bob（IDE ごと）を完全に終了して再起動**してください。再起動しないと Bob が Maven を認識しません。

---

## 3. V2 の注目機能

このラボで注目すべき Bob V2 の新機能です。

| 機能 | 説明 |
|------|------|
| **インタラクティブな承認フロー** | 各移行変更を Bob が提案し、適用前に明示的な承認を求めます。プレビューして確認してから適用できます |
| **Issue ごとのアップグレード** | 全レシピを一括適用せず、各 Issue を個別に対処します |
| **多層的な修正提案** | 必要に応じてライブラリレベルとランタイムレベルの両方の修正を提案します |
| **タスクごとのコスト内訳** | ワークフロー終了時にサブタスクごとの Bob コイン・トークン数を表示します |
| **ビジュアルなモダナイゼーションサマリー** | 実施した全ステップを結果ごとに分類したグラフィックを自動生成します |
| **ツールの自動インストール** | Java が未設定の場合、ワークフロー自身が SDKMAN! 経由でインストールします |

---

## 4. Java Modernization ワークフローの概要

IBM Bob の Java Modernization ワークフローは、3 つのフェーズで構成されています。

```
Analyze（分析）
 └─ プロジェクトを分析し、モダナイゼーションタイプを選択
        │
        ▼
Upgrade（アップグレード）
 └─ 選択したタイプに応じて、Issue を 1 件ずつ承認しながら自律的にアップグレード
        │
        ▼
Validate（検証）
 └─ アプリをビルド・デプロイして動作確認し、モダナイゼーションサマリーを生成
```

### Bob の権限設定

チャットウィンドウの権限セレクターで、Bob が自動実行できる操作を制御できます。

| 権限 | 内容 |
|------|------|
| **Read** | ファイルの読み取りを自動実行 |
| **Edit** | ファイルの編集を自動実行 |
| **Execute** | コマンドの実行を自動実行 |
| **MCP** | MCP の機能を自動実行 |

!!! tip "初めての方へ"
    最初は **Read のみ**を有効にして操作に慣れてから、徐々に権限を広げることをお勧めします。

---

## 5. セットアップ

### 0. スナップショットを取得する

Bob に以下をお願いしてください。

> 以下の URL から Lab 1 用のスナップショットをダウンロードして展開してください。
>
> `https://raw.githubusercontent.com/r-nakayamasan/bob-workshop-draft/main/snapshots/snapA-java-liberty-replatforming.zip`
>
> ダウンロードして `~/lab-workspace/` に展開してください。

Bob が自動で以下のコマンドを実行します（Mac/Windows どちらでも動作します）：

```bash
# ダウンロード（curl は Mac/Windows 10 以降に標準搭載）
curl -L -o snapA.zip "https://raw.githubusercontent.com/r-nakayamasan/bob-workshop-draft/main/snapshots/snapA-java-liberty-replatforming.zip"

# 展開（Python は Mac/Windows 共通で使用可能）
python -m zipfile -e snapA.zip ./lab-workspace/
```

!!! tip "作業フォルダの場所"
    展開先は任意の場所で構いません。`~/lab-workspace/` の部分はお好みのパスに変更してください。

### 1. スナップショットフォルダを IDE で開く

IBM Bob をインストールした IDE を起動し、展開した **`snapA-java-liberty-replatforming/`** フォルダをプロジェクトルートとして開いてください。

!!! warning "注意"
    `snapA-java-liberty-replatforming` フォルダをそのままルートとして開いてください。Java Modernization ワークフローはスナップショットフォルダを開いた場合にのみ表示されます。

### 2. Agent モードを確認する

Bob のチャットパネル下部のモードインジケーターが **Agent** になっていることを確認します。Agent は V2 のデフォルトモードです。

### 3. ワークフローが表示されることを確認する

Bob のチャットパネルで **Java Modernization** ワークフローが表示されていることを確認します。表示されない場合は、Bob の設定でチームメンバーシップを確認してください。

---

## 6. 演習 1：Liberty Replatforming ワークフローを実行する

### 目的

Bob の Java Modernization ワークフローを使い、薬局アプリを Traditional WebSphere から Liberty へ移行します。

### 手順

#### ステップ 1：ワークフローを開始する

1. Bob ウィンドウ上部の再生ボタン（▶）をクリックする
2. ワークフロー一覧から **Java Modernization** を選択する
3. **（▶）Start** をクリックする

#### ステップ 2：Analyze — プロジェクトを分析する

- **Project Path** フィールドに `snapA` フォルダのパスが自動入力されていることを確認します
- 「カスタムビルドコマンド」は空白のままにします
- **Continue** をクリックします

#### ステップ 3：Analyze — モダナイゼーションタイプを選択する

- **Liberty Replatforming** を選択します
- **Git Flow** をオフ（トグルをオフ）にします
- **Continue** をクリックします

#### ステップ 4：Upgrade — AMA 移行プランを指定する

以下の形式で移行プラン ZIP ファイルのフルパスを貼り付けます。

```
[ローカルパス]/Bobathon/labs/lab1-java-liberty-replatforming/simple-pharmacy.war_migrationPlan.zip
```

**Continue** をクリックします。Bob がプランを分析し、モダナイゼーション用の To-Do リストを作成します。

#### ステップ 5：Upgrade — インタラクティブな承認フロー

Bob は移行 Issue を 1 件ずつ特定し、以下の情報を提示します。

- 根本原因の説明
- 具体的な修正案（依存関係の変更や設定の追加など）
- 変更前の承認確認

典型的な選択肢：

| 選択肢 | 説明 |
|--------|------|
| `修正を承認して実装に進む` | そのまま適用 |
| `依存関係はまだ変更しない；詳しく確認する` | 保留して調査 |
| `修正を承認するが、適用前に正確な変更内容を見せてほしい` | diff を確認してから適用 |

!!! tip "推奨"
    最初の 1〜2 件は「**適用前に正確な変更内容を見せてほしい**」を選択して、Bob が実際に何を変更しようとしているか確認してみましょう。

**このプロジェクトで発生が予想される Issue**（実際には異なる場合があります）：

- **Javassist の文字コード警告**: 依存関係の除外 + アップグレード
  - `"PrintStream の出力ストリームでデフォルト文字コードを使用することを避ける" ルールの修正`
  - → `pom.xml の依存関係オーバーライドで javassist を 3.29.2-GA にアップグレード`
- **OGNL SecurityManager の競合**: 依存関係のアップグレード + JVM オプションファイル
  - → 誤検知の場合は「変更不要」を選択

#### ステップ 6：Upgrade — デプロイ

すべての修正が適用されると、ワークフローがアプリをビルドして Liberty にデプロイします。Bob はサーバーが起動したことと URL を報告します。

#### ステップ 7：Validate — モダナイゼーションサマリー ⭐ V2 新機能

- Bob が**ビジュアルなモダナイゼーションサマリー**を生成します。実施した全ステップが結果ごとに分類されます
- **タスクごとのコスト・トークン統計**も表示されます
- 「エラーなく正常に起動しました」と報告されることを確認します
- エラーがある場合は、エラー内容を Bob チャットに貼り付けてデバッグします

---

## 7. 演習 2：移行後のアプリを確認する

ブラウザで以下の URL を開きます。

```
http://localhost:9080/simple-pharmacy.war/
```

Bob がコンテキストルートを明示的に設定した場合（`context-root="/simple-pharmacy"`）：

```
http://localhost:9080/simple-pharmacy/dashboard
```

処方箋・注文・薬品データが表示された薬局ダッシュボードが表示されれば成功です。

以下の各ページも確認してみましょう。

```
http://localhost:9080/simple-pharmacy.war/dashboard
http://localhost:9080/simple-pharmacy.war/prescription-list.action
http://localhost:9080/simple-pharmacy.war/order-list.action
http://localhost:9080/simple-pharmacy.war/medicine-list.action
```

すべてのページが正しく表示されれば、移行は完了です。

---

## 8. トラブルシューティング

### Maven インストール後にコマンドが見つからない

**症状**: `mvn: command not found`

**対処法**:

1. SDKMAN! のインストールを確認: `sdk version`
2. Maven を再インストール: `sdk install maven`
3. Bob を完全に終了して再起動
4. 新しいターミナルで確認: `mvn --version`

---

### Bob のターミナルで Java バージョンが異なる

**症状**: Bob のターミナルで `java -version` を実行すると、期待と異なるバージョンが表示される

**対処法**: `sdk use java <識別子>` はそのシェルにしか適用されません。Bob のターミナルで `sdk use java 8.0.492-zulu` を実行するか、`sdk default java 8.0.492-zulu` でグローバルデフォルトを設定してください。

---

### Bob がプロジェクトファイルを読み取れない

**症状**: 「そのファイルにアクセスできません」または「ファイルが見つかりません」

**対処法**:

1. `snapA-java-liberty-replatforming` サブフォルダをプロジェクトルートとして開いているか確認
2. ファイルのパーミッションを確認: `ls -la`
3. Bob にワークスペースへの読み取り権限があることを確認
4. `@ファイル名` 構文でファイルを参照してみる

---

### JAR ファイルにアクセスできない

**症状**:

```
Error: Unable to access jarfile /Applications/IBM Bob.app/.../ta-jam-2.2.1.jar
```

**対処法**:

1. `/Applications/IBM Bob.app/Contents/Resources/app/extensions/bob-code/assets/` に `jars` フォルダを作成
2. 以下の構成で JAR ファイルを配置:
   ```
   jars/
   ├── prompt-lib/
   │   └── prompt-lib-2.2.0.jar
   └── ta-jam-2.2.1.jar
   ```
3. Bob を閉じて再起動

!!! warning "注意"
    Bob が `.dmg` からマウントした `/Volumes` から実行されている場合はフォルダやファイルの作成が拒否されます。Bob アプリを `/Applications` フォルダに移動してから起動してください。

---

### `SRVE0321E: The [struts2] filter did not load during start up`

**対処法**: 無視して問題ありません。移行済みアプリは正常に動作します。このメッセージはクラスローディングの副作用によるもので、ランタイムの動作には影響しません。

---

### ワークフローが変更を提案したが内容が不明

**対処法**: Bob が「適用前に正確な変更内容を見せてほしい」オプションを提示する場合は必ず選択してください。ファイルに変更が加わる前に完全な diff を確認できます。

---

## 9. まとめ

おめでとうございます！Bob V2 の Java Modernization ワークフローを使って Liberty Replatforming ラボを完了しました。

- ✅ 前提条件のセットアップ（SDKMAN!、Java、Maven）
- ✅ Bob V2 の Java Modernization ワークフローの起動
- ✅ 各移行変更に対するインタラクティブな承認フローの操作
- ✅ Bob のタスクごとのコスト・ビジュアルモダナイゼーションサマリーの読み取り
- ✅ 移行済み Liberty アプリケーションのデプロイと動作確認

次は **[Lab 2 — Java 21 へのアップグレード](java_premium_lab2.md)** に進みましょう。
