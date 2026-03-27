# 🏛️ インタラクティブ博物館 — Interactive Museum

> **C言語とアセンブラの世界を体験する、8つのインタラクティブな展示室**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](https://github.com/yourrepo/pulls)
[![Built by gstate.dev](https://img.shields.io/badge/built%20by-gstate.dev-blue)](https://gstate.dev/)

---

## 🗺️ 概要

**インタラクティブ博物館**は、C言語とアセンブリ言語の世界を**8つの展示室**を通じて体験・学習・探求するためのオープンソースプロジェクトです。

コードの歴史・構造・脆弱性・モダナイズまでを、実際に動かしながら学べる**ハンズオン型の学習プラットフォーム**です。

```
Interactive Museum
├── 展示室 1 ── C ⇒ アセンブラ コンバーター
├── 展示室 2 ── データ生成パイプライン
├── 展示室 3 ── ロゼッタストーン検索アプリ
├── 展示室 4 ── アノテーション協力ゲーム
├── 展示室 5 ── VS Code 拡張
├── 展示室 6 ── 可視化 Web アプリ
├── 展示室 7 ── 脆弱性ハンティング CLI
└── 展示室 8 ── モダナイズパイプライン
```

---

## 🏛️ 展示室ガイド

---

### 展示室 1 — C ⇒ アセンブラ コンバーター

> **「機械語の扉を開く」**

C言語のソースコードをリアルタイムでアセンブリ言語に変換し、対応関係を視覚的にハイライト表示します。

#### 機能
- C コードとアセンブリコードの**行単位マッピング**
- x86_64 / ARM64 / RISC-V アーキテクチャ対応
- 最適化レベル（`-O0` 〜 `-O3`）切り替え
- レジスタ使用状況のリアルタイムトレース
- インライン解説（各命令の日本語注釈）

#### ディレクトリ構成

```
01-c-to-asm/
├── src/
│   ├── compiler.py        # GCC/Clang ラッパー
│   ├── mapper.py          # 行マッピングロジック
│   └── highlighter.py     # シンタックスハイライト
├── frontend/
│   ├── index.html
│   └── editor.js
├── tests/
└── README.md
```

#### クイックスタート

```bash
cd 01-c-to-asm
pip install -r requirements.txt
python src/compiler.py --input sample.c --arch x86_64 --opt O2
```

---

### 展示室 2 — データ生成パイプライン

> **「学習データを錬金する」**

C言語コードとアセンブリコードのペアデータセットを大規模に生成するパイプラインです。機械学習モデルの学習・評価データとして利用できます。

#### 機能
- **合成コード生成**（テンプレートベース＋LLM補完）
- 複数コンパイラ対応（GCC / Clang / MSVC）
- アーキテクチャ・最適化レベルのクロス生成
- Parquet / JSONL / CSV 出力
- Apache Airflow / Prefect によるワークフロー管理
- データ品質スコアリング（重複排除・フィルタリング）

#### ディレクトリ構成

```
02-data-pipeline/
├── generators/
│   ├── synthetic.py       # 合成コード生成
│   ├── compiler_farm.py   # マルチコンパイラ実行
│   └── quality_filter.py  # 品質スコアリング
├── workflows/
│   ├── airflow_dag.py
│   └── prefect_flow.py
├── schemas/
│   └── dataset_schema.json
├── output/                # 生成データ出力先
└── README.md
```

#### クイックスタート

```bash
cd 02-data-pipeline
docker-compose up -d
python generators/synthetic.py --count 10000 --arch all --output ./output
```

---

### 展示室 3 — ロゼッタストーン検索アプリ

> **「コードの翻訳辞書を引く」**

「このCのパターンはアセンブリでどう書く？」という疑問に即答する**意味検索エンジン**です。コードの断片を入力すると、対応するアセンブリパターンを検索・提示します。

#### 機能
- **セマンティック検索**（コード埋め込みベクトルによる類似検索）
- C スニペット → アセンブリパターン の双方向検索
- ファジー検索・正規表現フィルタ対応
- 検索履歴・お気に入り保存
- REST API / Web UI 両対応

#### ディレクトリ構成

```
03-rosetta-search/
├── backend/
│   ├── api/
│   │   ├── main.py        # FastAPI エンドポイント
│   │   └── search.py      # 検索ロジック
│   ├── embeddings/
│   │   ├── encoder.py     # コード埋め込み生成
│   │   └── index.py       # ベクトルインデックス（FAISS）
│   └── db/
│       └── models.py
├── frontend/
│   ├── src/
│   └── public/
├── scripts/
│   └── build_index.py     # インデックス構築
└── README.md
```

#### クイックスタート

```bash
cd 03-rosetta-search
pip install -r requirements.txt
python scripts/build_index.py --dataset ../02-data-pipeline/output
uvicorn backend.api.main:app --reload
```

---

### 展示室 4 — アノテーション協力ゲーム

> **「みんなでコードに注釈をつける」**

C ↔ アセンブリのペアデータに対して、複数ユーザーが協力してアノテーション（注釈・対応ラベル）をつけるゲーミフィケーション付きWebアプリです。

#### 機能
- タスクキュー管理（ランダム・難易度別割り当て）
- **リアルタイム協力モード**（WebSocket同期）
- アノテーション品質スコア（Inter-Annotator Agreement）
- ポイント・バッジ・ランキングシステム
- エクスポート機能（アノテーション済みデータセット出力）

#### ディレクトリ構成

```
04-annotation-game/
├── server/
│   ├── app.py             # Flask / FastAPI サーバー
│   ├── websocket.py       # リアルタイム同期
│   ├── scoring.py         # IAA スコアリング
│   └── gamification.py    # ポイント・バッジロジック
├── client/
│   ├── src/
│   │   ├── components/
│   │   ├── game/
│   │   └── App.tsx
│   └── public/
├── database/
│   └── migrations/
└── README.md
```

#### クイックスタート

```bash
cd 04-annotation-game
docker-compose up -d postgres redis
pip install -r requirements.txt
python server/app.py
cd client && npm install && npm run dev
```

---

### 展示室 5 — VS Code 拡張

> **「エディタの中に博物館を」**

Visual Studio Code 上で C言語コードを書きながら、対応するアセンブリをリアルタイムプレビューできる拡張機能です。

#### 機能
- **インラインアセンブリプレビュー**（サイドパネル表示）
- カーソル位置の C行 ↔ ASM行 相互ジャンプ
- ホバー時のアセンブリ命令解説（日本語）
- ロゼッタストーン検索の統合（Ctrl+Shift+R）
- 脆弱性パターン警告（展示室7と連携）
- 設定：アーキテクチャ・最適化レベル・テーマ選択

#### ディレクトリ構成

```
05-vscode-extension/
├── src/
│   ├── extension.ts       # エントリポイント
│   ├── asmPreview.ts      # アセンブリプレビュー
│   ├── lineMapper.ts      # 行マッピング
│   ├── hoverProvider.ts   # ホバー解説
│   └── rosettaClient.ts   # 検索API連携
├── webview/
│   ├── preview.html
│   └── preview.css
├── test/
├── package.json
└── README.md
```

#### クイックスタート

```bash
cd 05-vscode-extension
npm install
npm run compile
# VS Code で F5 → 拡張機能開発ホストが起動
```

#### インストール（VSIX）

```bash
npm run package
code --install-extension interactive-museum-0.1.0.vsix
```

---

### 展示室 6 — 可視化 Web アプリ

> **「コードを絵で読む」**

C言語・アセンブリのデータセットや解析結果を、インタラクティブなグラフ・チャートで可視化するダッシュボードです。

#### 機能
- **命令頻度ヒートマップ**（アーキテクチャ・最適化レベル別）
- C構文パターンとASM命令の対応サンドバー（Sankey図）
- データセット統計ダッシュボード（件数・品質スコア・分布）
- アノテーション進捗トラッカー（展示室4連携）
- タイムライン：コンパイラバージョン別の最適化進化
- フィルタ・ドリルダウン・CSV/PNG エクスポート

#### ディレクトリ構成

```
06-visualization/
├── src/
│   ├── components/
│   │   ├── HeatMap.tsx
│   │   ├── SankeyChart.tsx
│   │   ├── Dashboard.tsx
│   │   └── Timeline.tsx
│   ├── hooks/
│   ├── api/
│   └── App.tsx
├── public/
├── vite.config.ts
└── README.md
```

#### 技術スタック

| レイヤー | 技術 |
|---------|------|
| フロントエンド | React + TypeScript |
| グラフライブラリ | D3.js / Recharts / Observable Plot |
| ビルドツール | Vite |
| スタイル | Tailwind CSS |

#### クイックスタート

```bash
cd 06-visualization
npm install
npm run dev
# → http://localhost:5173
```

---

### 展示室 7 — 脆弱性ハンティング CLI

> **「コードの地雷を探せ」**

C言語コードベースを静的解析し、アセンブリレベルでの脆弱性パターンを検出するコマンドラインツールです。

#### 機能
- **バッファオーバーフロー**検出（スタック・ヒープ）
- **Use-After-Free / Double-Free** パターン認識
- **整数オーバーフロー**・符号付き/なし変換バグ
- **フォーマット文字列脆弱性**スキャン
- ASM レベルの根拠提示（問題箇所のアセンブリ表示）
- SARIF / JSON / HTML レポート出力
- CI/CD 統合（GitHub Actions / GitLab CI）
- VS Code拡張（展示室5）との連携

#### ディレクトリ構成

```
07-vuln-hunter/
├── src/
│   ├── scanner/
│   │   ├── main.py        # CLIエントリポイント
│   │   ├── analyzer.py    # 静的解析エンジン
│   │   ├── patterns/
│   │   │   ├── bof.py     # バッファオーバーフロー
│   │   │   ├── uaf.py     # Use-After-Free
│   │   │   ├── integer.py # 整数系脆弱性
│   │   │   └── format.py  # フォーマット文字列
│   │   └── asm_tracer.py  # ASMレベル解析
│   └── reporters/
│       ├── sarif.py
│       ├── json_report.py
│       └── html_report.py
├── rules/                 # 検出ルール定義（YAML）
├── tests/
│   └── vulnerable_samples/ # テスト用脆弱コード
└── README.md
```

#### クイックスタート

```bash
cd 07-vuln-hunter
pip install -r requirements.txt

# 単一ファイルスキャン
python src/scanner/main.py scan --input target.c

# ディレクトリ一括スキャン
python src/scanner/main.py scan --input ./project --recursive --report sarif

# CIモード（脆弱性検出時に非ゼロ終了）
python src/scanner/main.py scan --input ./project --ci --threshold high
```

#### 使用例

```
$ python src/scanner/main.py scan --input vulnerable.c

🔍 Scanning: vulnerable.c
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
⚠ [HIGH] Buffer Overflow detected
  Line 42: strcpy(buf, input)
  ASM: mov %rdi, %rsi → call strcpy@plt
  Reason: 宛先バッファサイズ未チェック

⚠ [MEDIUM] Use-After-Free risk
  Line 78: ptr->field = value
  ASM: mov %rax, (%rdx)
  Reason: free() 後のポインタ参照
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Found: 2 issues (1 HIGH, 1 MEDIUM)
```

---

### 展示室 8 — モダナイズパイプライン

> **「古いコードを現代へ連れ出す」**

レガシーC言語コードを解析し、モダンなC++・Rust・Go などへの移行を支援する自動変換パイプラインです。

#### 機能
- **AST解析**による構造理解（libclang / tree-sitter）
- **自動リファクタリング提案**（安全なメモリ管理・モダン構文）
- C → C++17/20 変換（スマートポインタ・RAII・STL活用）
- C → Rust 変換支援（所有権・ライフタイム注釈生成）
- C → Go 変換支援（goroutine・チャネル対応）
- **差分レポート**：変換前後のアセンブリ比較
- 変換品質スコアリング・テスト生成

#### ディレクトリ構成

```
08-modernize-pipeline/
├── src/
│   ├── parser/
│   │   ├── ast_extractor.py   # libclang AST抽出
│   │   └── tree_sitter.py     # tree-sitter パーサー
│   ├── analyzers/
│   │   ├── memory_patterns.py # メモリ管理パターン認識
│   │   └── complexity.py      # 複雑度解析
│   ├── transformers/
│   │   ├── cpp_transformer.py # C → C++ 変換
│   │   ├── rust_transformer.py# C → Rust 変換
│   │   └── go_transformer.py  # C → Go 変換
│   ├── validators/
│   │   ├── asm_diff.py        # アセンブリ差分検証
│   │   └── test_generator.py  # テスト自動生成
│   └── pipeline.py            # パイプライン統括
├── configs/
│   └── rules/                 # 変換ルール定義
├── examples/
│   ├── input/                 # 変換前サンプル
│   └── output/                # 変換後サンプル
└── README.md
```

#### クイックスタート

```bash
cd 08-modernize-pipeline
pip install -r requirements.txt

# C → Rust 変換
python src/pipeline.py \
  --input legacy_code.c \
  --target rust \
  --validate asm-diff \
  --output ./output

# バッチ変換（プロジェクト全体）
python src/pipeline.py \
  --input ./legacy_project \
  --target cpp \
  --recursive \
  --report html
```

---

## 🚀 プロジェクト全体のセットアップ

### 前提条件

| ツール | バージョン | 用途 |
|--------|-----------|------|
| Python | 3.11+ | バックエンド全般 |
| Node.js | 20+ | フロントエンド・VS Code拡張 |
| GCC / Clang | 14+ | コンパイル・アセンブリ生成 |
| Docker | 24+ | パイプライン・DB |
| Rust | 1.75+ | モダナイズターゲット（展示室8） |

### 全展示室の一括起動

```bash
# リポジトリをクローン
git clone https://github.com/yourrepo/interactive-museum.git
cd interactive-museum

# 依存関係インストール
make install

# 全展示室を起動（Docker Compose）
docker-compose up -d

# 個別に起動
make start ROOM=01   # C⇒ASMコンバーター
make start ROOM=03   # ロゼッタストーン検索
make start ROOM=06   # 可視化Webアプリ
```

### ディレクトリ全体構成

```
interactive-museum/
├── 01-c-to-asm/
├── 02-data-pipeline/
├── 03-rosetta-search/
├── 04-annotation-game/
├── 05-vscode-extension/
├── 06-visualization/
├── 07-vuln-hunter/
├── 08-modernize-pipeline/
├── shared/
│   ├── models/            # 共通データモデル
│   ├── utils/             # 共通ユーティリティ
│   └── schemas/           # 共通スキーマ定義
├── docker-compose.yml
├── Makefile
├── .github/
│   └── workflows/
│       ├── ci.yml
│       └── release.yml
└── README.md              # このファイル
```

---

## 🔗 展示室の連携図

```
         ┌─────────────────────────────┐
         │  展示室2: データ生成パイプライン   │
         │  (C/ASMペアデータセット生成)      │
         └──────────┬──────────────────┘
                    │ データセット
         ┌──────────▼──────────┐
         │  展示室3: ロゼッタ検索  │ ◄── 展示室5 (VS Code拡張)
         │  (意味検索エンジン)    │
         └──────────┬──────────┘
                    │ 検索結果
    ┌───────────────┼───────────────┐
    ▼               ▼               ▼
展示室1          展示室4          展示室6
C⇒ASM変換    アノテーション      可視化
コンバーター    協力ゲーム       Webアプリ
                    │
                    │ アノテーション済みデータ
                    ▼
             展示室7              展示室8
          脆弱性ハンティング     モダナイズ
              CLI               パイプライン
               │
               └──────────────► 展示室5 (VS Code 警告表示)
```

---

## 🛠️ 開発ガイド

### コントリビューション

1. このリポジトリをフォーク
2. フィーチャーブランチを作成（`git checkout -b feature/room-XX-your-feature`）
3. 変更をコミット（`git commit -m 'feat: Add feature to Room XX'`）
4. ブランチにプッシュ（`git push origin feature/room-XX-your-feature`）
5. プルリクエストを作成

### コーディング規約

- Python: [PEP 8](https://peps.python.org/pep-0008/) + [Black](https://black.readthedocs.io/) フォーマッター
- TypeScript/JavaScript: ESLint + Prettier
- コミットメッセージ: [Conventional Commits](https://www.conventionalcommits.org/)

### テスト実行

```bash
# 全展示室のテスト
make test

# 個別展示室のテスト
make test ROOM=07

# カバレッジレポート
make coverage
```

---

## 📄 ライセンス

このプロジェクトは **MIT ライセンス** の下で公開されています。詳細は [LICENSE](./LICENSE) を参照してください。

---

## 🌐 関連リソース

- 🔗 **プロジェクトサイト**: [https://gstate.dev/](https://gstate.dev/)
- 📦 **データセット**: Hugging Face Hub（準備中）
- 📝 **ドキュメント**: [https://gstate.dev/docs](https://gstate.dev/docs)
- 💬 **Discussions**: GitHub Discussions
- 🐛 **バグ報告**: GitHub Issues

---

<div align="center">

**Built with ❤️ by [gstate.dev](https://gstate.dev/)**

*C言語とアセンブラの世界を、すべての人に。*

</div>
