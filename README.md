# bob-workshop

MkDocs Materialを使用したドキュメントサイトプロジェクト

## 必要要件

- Python 3.12以上
- uv (Pythonパッケージマネージャー)

## セットアップ

### 1. uvのインストール

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

### 2. 依存関係のインストール

```bash
uv sync
```

## ローカル起動方法

### 開発サーバーの起動

```bash
uv run mkdocs serve
```

ブラウザで `http://127.0.0.1:8000` にアクセスしてドキュメントを確認できます。

ファイルを編集すると、自動的にブラウザがリロードされます。

### Pythonスクリプトの実行

```bash
uv run python main.py
```

## デプロイ方法

### GitHub Pagesへのデプロイ

```bash
uv run mkdocs gh-deploy
```

このコマンドは以下を自動的に実行します：
1. ドキュメントサイトをビルド
2. `gh-pages`ブランチを作成/更新
3. GitHub Pagesにデプロイ

### 静的ファイルのビルド

他のホスティングサービスにデプロイする場合は、まず静的ファイルをビルドします：

```bash
uv run mkdocs build
```

ビルドされたファイルは`site/`ディレクトリに出力されます。

## プロジェクト構成

```
bob-workshop/
├── docs/              # ドキュメントのMarkdownファイル
│   └── index.md      # ホームページ
├── mkdocs.yml        # MkDocsの設定ファイル
├── main.py           # Pythonスクリプト
├── pyproject.toml    # プロジェクト設定
└── README.md         # このファイル
```

## 開発

### 新しいページの追加

1. `docs/`ディレクトリに新しいMarkdownファイルを作成
2. `mkdocs.yml`のnavセクションに追加（オプション）

### 設定のカスタマイズ

[`mkdocs.yml`](mkdocs.yml)ファイルでサイト名、テーマ、ナビゲーションなどを設定できます。

詳細は[MkDocs公式ドキュメント](https://www.mkdocs.org)を参照してください。