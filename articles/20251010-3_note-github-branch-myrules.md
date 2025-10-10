---
title: "【GitHub】ブランチ名のマイルール"
emoji: "🐈"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["github", "git", "開発効率化"]
published: true
---
## はじめに
チーム開発でも個人開発でも、ブランチ名の付け方って意外と悩みどころですよね。  
思いつくままに名前を付けていると、あとで自分でも「これ何のブランチだっけ？」となりがちです。
この記事では、私が実際の案件で運用している**GitHubブランチ名のマイルール**を紹介します🌿

## 基本方針
- **目的が一目でわかること**  
- **誰でも迷わず付けられること**  
- **ブランチ一覧が整理されること**
```bash
# NG例（目的が不明）
test
fix1
branch2

# OK例（目的が明確）
feature/add-login
fix/typo-homepage
docs/update-readme
```

## 命名の基本構成
```bash
<種類>/<内容>
```
### 種類部分のルール
| 種類 | 用途 |
|------|------|
| `feature/` | 新機能追加 |
| `fix/` | 不具合修正 |
| `hotfix/` | 緊急修正（本番環境） |
| `refactor/` | リファクタリング |
| `docs/` | ドキュメント修正 |
| `style/` | コードスタイル・デザイン修正 |
| `chore/` | その他の微調整・環境設定 |

### 内容部分のルール
- 英語で簡潔に書く（例：add-header-nav）
- 動詞から始める（add-, update-, remove-, fix- など）
- ケバブケース（ハイフン区切り）で統一 ※スネークケースなどと混在しない
- 短くても意味が伝わることを優先
```bash
feature/add-contact-form
fix/spelling-error-in-footer
refactor/user-controller
```

**実際に使っている例**
feature/add-static-pages
feature/public-ui
fix/delete-confirm
refactor/form-component
docs/update-readme

**簡単に解説すると👇**
feature/add-static-pages：静的ファイルを追加
fix/delete-confirm：削除確認モーダルの修正
docs/update-readme：READMEの更新

## まとめ
ブランチ名の付け方は、地味なようで開発の整理整頓に大きく影響します。
命名ルールを一度決めてしまえば、チームでも個人でも「どのブランチで何をしているのか」が一目でわかるようになります。
自分なりのマイルールを持って、より快適なGitHubライフを送りましょう🌿