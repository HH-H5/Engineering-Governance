---
doc_id: GOV-CHANGE-001
tier: 1
status: approved
authority: canonical
owner: human
change_policy: human-approval-required
---

# Change Control

## 1. Purpose

この文書は、上位ガバナンス文書を安全に変更するための基本ルールを定義する。

Tier 1は複数のプロジェクトへ影響するため、通常の実装ドキュメントより慎重に変更する。

---

## 2. Human Approval

Tier 1の確定変更には、人間の明示的な承認を必要とする。

AIはTier 1の変更案を作成できるが、

```text
提案
```

と

```text
承認されたルール
```

を区別しなければならない。

AIが変更案を作成しただけでは、既存ルールは変更されたことにならない。

---

## 3. Review Before Publication

Tier 1を公開リポジトリへ追加または変更する前に、変更内容を確認する。

特に次を確認する。

- 秘密情報が含まれていない
- 個人情報が含まれていない
- 個別プロジェクトの不要な内部情報が含まれていない
- 未公開事業情報が含まれていない
- 外部公開する必要性がある
- 既存Tier 1と矛盾しない

公開可否に疑問がある場合は、公開しない。

---

## 4. Separate Governance Changes

可能な限り、Tier 1の変更と通常の製品実装変更を分離する。

Tier 1の変更は、その変更自体をレビューできる単位にする。

これにより、

```text
製品実装を通すために、
気付かないうちにガバナンスまで変更された
```

という状態を避ける。

---

## 5. Change Proposal

Tier 1変更案には、可能な限り次の情報を含める。

```text
Current Rule:
現在のルール

Problem:
何が問題なのか

Proposed Change:
どう変更するのか

Reason:
なぜ必要なのか

Impact:
どの種類のプロジェクトや作業へ影響するか

Migration:
既存プロジェクト側で対応が必要か
```

小さな文章修正など、意味を変更しない変更については簡略化できる。

---

## 6. Semantic Changes

文言だけでなく、意味が変更される場合は、それを明示する。

例:

```text
Before:
AIはTier 2を変更できない。

After:
AIはTier 2の変更PRを作成できるが、人間承認なしに確定できない。
```

このような変更は単なる表現修正として扱わない。

---

## 7. Conflict Discovery

Tier 1内部に矛盾が発見された場合、AIは独自判断で一方を削除または上書きしてはならない。

矛盾内容を人間へ提示し、解決方針の承認を得る。

解決までは、より安全で不可逆性の低い行動を優先する。

---

## 8. Project Adoption

Tier 1を更新したからといって、すべてのプロジェクトが即座に新しいルールへ移行する必要はない。

各プロジェクトがTier 1の特定バージョンまたは特定commitを参照できる構成を推奨する。

これにより、

```text
Governance更新
↓
プロジェクト側で内容確認
↓
明示的に新Versionへ更新
```

という移行が可能になる。

---

## 9. History

Tier 1の過去の変更理由を追跡できる状態を維持する。

Gitの履歴、Pull Request、Commit Messageなどを利用し、

> なぜこのルールが存在するのか

を後から確認できるようにする。

---

## 10. Public Repository Rule

公開リポジトリでは、削除後もGit履歴に情報が残る可能性がある。

そのため、

```text
まずcommitして、問題があれば後で消す
```

という運用を行わない。

公開前に内容を確認し、安全性を確認したものだけをcommitする。
