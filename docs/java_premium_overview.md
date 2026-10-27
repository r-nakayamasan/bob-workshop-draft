# Java Modernization ワークショップ — 概要

IBM Bob を使った Java アプリケーションのモダナイゼーションを、段階的に体験するハンズオンワークショップです。

!!! info "このワークショップについて"
    実習対象アプリケーションは **Simple Pharmacy Management System**（薬局管理システム）です。処方箋・注文・薬品在庫・ダッシュボードを管理するシンプルな Web アプリケーションを使い、Traditional WebSphere + Java 8 + Struts の構成から段階的にモダナイズします。

---

## モダナイゼーションの全体像

### 各ラボでの変化点

| ラボ | テーマ | アプリサーバー | Java | フロントエンド | テスト | セキュリティ |
|------|--------|:-----------:|:----:|:----------:|:----:|:--------:|
| **スタート** | — | WebSphere | 8 | Struts | — | — |
| **Lab 1** | Liberty への移行 | **Liberty** ✅ | 8 | Struts | — | — |
| **Lab 2** | Java アップグレード | Liberty | **21** ✅ | Struts | — | — |
| **Lab 3** | UI モダナイゼーション | Liberty | 21 | **React** ✅ | — | — |
| **Lab 4** | ユニットテスト生成 | Liberty | 21 | React | **80%+** ✅ | — |
| **Lab 5** | 脆弱性の修正 | Liberty | 21 | React | 80%+ | **強化済** ✅ |

---

## ラボ一覧

| ラボ | テーマ | 使用するワークフロー | 所要時間 |
|-----|--------|-------------------|---------|
| **Lab 1** | WebSphere → Liberty への移行 | Java Modernization（Liberty Replatforming） | 45〜60 分 |
| **Lab 2** | Java 8 → Java 21 アップグレード | Java Modernization（Java Upgrade） | 45〜60 分 |
| **Lab 3** | Struts → React へのUI移行 | Java Modernization（UI Modernization） | 60〜90 分 |
| **Lab 4** | ユニットテストの自動生成 | Java Unit Testing | 60〜75 分 |
| **Lab 5** | 依存ライブラリの脆弱性検出と修正 | Java Vulnerabilities Detection | 60〜75 分 |

---

## 推奨受講パス

### パス 1：順番にすべてのラボを受講する（推奨）

Java アプリケーションのモダナイゼーション全体像を体験したい方向けです。

1. Lab 1 → Lab 2 → Lab 3 → Lab 4 → Lab 5

**合計時間**: 約 5〜7 時間

### パス 2：興味のあるラボだけ受講する

各ラボは独立して実施できます。特定のテーマに集中したい場合に最適です。

- Liberty への移行に興味がある → **Lab 1**
- Java バージョンアップに興味がある → **Lab 2**
- フロントエンドのモダナイゼーションに興味がある → **Lab 3**
- テスト自動生成に興味がある → **Lab 4**
- セキュリティ対策に興味がある → **Lab 5**

---

## スナップショットの仕組み

各ラボには **スナップショット（Snapshot）** と呼ばれる、作業開始時点のコードが用意されています。

| スナップショット | 対応ラボ | 内容 |
|---------------|---------|------|
| `snapA-java-liberty-replatforming` | Lab 1 | Traditional WebSphere + Java 8 + Struts |
| `snapB-java-upgrade` | Lab 2 | Liberty + Java 8 + Struts |
| `snapC-ui-mod` | Lab 3 | Liberty + Java 21 + Struts |
| `snapD-unit-test-gen` | Lab 4 | Liberty + Java 21 + React（テストなし） |
| `snapE-security-vulnerabilities` | Lab 5 | Liberty + Java 21 + React（脆弱性あり） |

各スナップショットは ZIP ファイルとして GitHub に公開されています。各ラボの「セットアップ」セクションに記載されている手順に従い、**Bob に依頼してダウンロード・展開**してから IDE で開いてください。

!!! info "Bob への依頼方法（例）"
    ```
    以下の URL からスナップショットをダウンロードして展開してください。
    https://raw.githubusercontent.com/r-nakayamasan/bob-workshop-draft/main/snapshots/snapA-java-liberty-replatforming.zip
    ~/lab-workspace/ に展開してください。
    ```
    Bob が `curl` と `python -m zipfile` コマンドを自動実行します（Mac/Windows 共通）。

---

## 前提ツールの確認

ハンズオンを始める前に、以下がインストールされているか確認してください。

```bash
# Java
java -version

# Maven
mvn --version

# Node.js（Lab 3 のみ必要）
node --version
npm --version
```

詳細なセットアップ手順は「[準備編](java_premium_00_preparation.md)」を参照してください。

---

## 次のステップ

準備ができたら、[Lab 1 — Liberty への移行](java_premium_lab1.md)から始めましょう。
