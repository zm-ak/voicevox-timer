# AI活用開発ルール・フロー（決定版 v1.0）

## 目的

本プロジェクトでは、AIを活用して以下を実現する。

- 開発速度の向上
- ドキュメントと実装の同期維持
- コンテキスト管理の効率化
- 長期開発における設計意図の保持
- AIツールの乗り換え耐性確保
- コスト最適化

---

# 基本方針

## AIの役割

### Gemini

担当：

- 要件整理
- 設計
- 技術選定
- ADR作成
- ドキュメント更新
- 調査
- 情報整理

### Claude Code

担当：

- 実装
- リファクタリング
- テスト
- コードレビュー
- 大規模改修

### Cursor

担当：

- メインIDE
- コード編集
- モック開発
- Agent実行
- 日常開発

### NotebookLM

担当：

- ドキュメント検索
- 設計判断の参照
- 過去経緯の確認

※ NotebookLMは知識検索用途のみとし、開発コンテキストの正本として利用しない。

---

# 正本管理

## 唯一の正本

以下を唯一の正本とする。

```text
Git Repository

docs/
adr/
AGENTS.md
```

AIの記憶やNotebookLMの内容は正本ではない。

---

# リポジトリ構成

```text
repo/

├ src/

├ docs/
│
├ requirements.md
├ architecture.md
├ api.md
├ db.md
├ changelog.md
│
└ adr/
    ├ 0001-xxxx.md
    ├ 0002-xxxx.md
    └ ...

├ AGENTS.md

├ .cursor/
│   └ rules/

├ scripts/

└ README.md
```

---

# ドキュメント定義

## requirements.md

目的：

- 何を作るか

内容：

- 背景
- 目的
- ユーザー
- 機能一覧
- 非機能要件

---

## architecture.md

目的：

- システム全体設計

内容：

- システム構成
- レイヤ構成
- 採用技術
- データフロー

---

## api.md

目的：

- API仕様管理

内容：

- エンドポイント
- リクエスト
- レスポンス
- エラー仕様

---

## db.md

目的：

- DB設計管理

内容：

- テーブル
- カラム
- リレーション

---

## changelog.md

目的：

- 変更履歴管理

内容：

- 機能追加
- 修正内容
- リリース内容

---

## adr/

目的：

- 設計判断の記録

内容：

- 検討内容
- 採用案
- 不採用案
- 採用理由

例：

```text
0001-authentication.md

認証方式

候補
- Session
- JWT

採用
- JWT

理由
- モバイル対応を考慮
```

---

# 開発フロー

## Step1 要件整理

Geminiを利用する。

成果物：

```text
requirements.md
```

---

## Step2 設計

Geminiを利用する。

成果物：

```text
architecture.md
api.md
db.md
```

---

## Step3 ADR作成

Geminiを利用する。

成果物：

```text
docs/adr/*
```

---

## Step4 モック開発

Cursor + Claudeを利用する。

実装前に確認すること：

```text
requirements.md
architecture.md
adr/*
```

---

## Step5 本実装

Claude Codeを利用する。

入力：

```text
requirements.md
architecture.md
adr/*
```

実装対象：

- フロントエンド
- バックエンド
- API
- DB
- テスト

---

## Step6 ドキュメント更新

Geminiを利用する。

入力：

```text
git diff
```

更新対象：

```text
api.md
architecture.md
db.md
changelog.md
```

必要に応じてADRを追加する。

---

## Step7 NotebookLM同期

同期対象：

```text
docs/*
README.md
```

目的：

- 設計検索
- 開発履歴検索
- 判断経緯検索

---

# Definition of Done

タスク完了条件

以下を全て満たした場合のみ完了とする。

```text
□ 実装完了

□ テスト成功

□ ドキュメント更新

□ CHANGELOG更新

□ ADR確認

□ Git Commit完了
```

---

# AGENTS運用方針

AIへの説明を毎回繰り返さない。

共通ルールはAGENTS.mdへ記載する。

---

## AGENTS.mdに記載する内容

```text
実装前

- requirements.mdを読む
- architecture.mdを読む
- adrを読む

実装後

- テスト
- docs更新
- changelog更新

禁止事項

- anyの利用
- ドキュメント未更新で完了扱い
- 設計方針を無視した実装
```

---

# Cursor Rules

Project Rulesに以下を設定する。

```text
必ずAGENTS.mdを読む

必ずdocsを参照する

必ずADRを参照する

ドキュメント更新を忘れない

CHANGELOGを更新する
```

---

# scripts運用

AIに作業させるための共通入口を作成する。

例：

```text
scripts/

create-feature.sh

update-docs.sh

release.sh
```

AIへはスクリプト実行を指示する。

---

# NotebookLM運用ルール

NotebookLMは検索専用とする。

利用用途：

- 設計確認
- API確認
- 過去判断確認

禁止：

- NotebookLM要約を正本として扱う
- NotebookLMのみをコンテキストとして実装する

---

# 長期方針

AIツールは変更可能とする。

以下は交換可能な構成要素である。

- Cursor
- Claude Code
- Gemini
- 将来のAIエージェント

一方で以下は変更しない。

```text
Git

docs/

ADR

AGENTS.md
```

プロジェクトの知識はツールではなく成果物に蓄積する。

---

# 現在の推奨構成

## AI

- Gemini：設計・ドキュメント
- Claude Code：実装
- NotebookLM：知識検索

## IDE

- Cursor

## 知識管理

- docs/
- ADR
- AGENTS.md

## ソース管理

- Git
- GitHub

## 基本原則

「AIに知識を持たせる」のではなく、

「成果物に知識を蓄積し、AIに読ませる」

ことを前提とする。