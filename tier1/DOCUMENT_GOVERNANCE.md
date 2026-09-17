---
doc_id: GOV-DOC-001
tier: 1
status: approved
authority: canonical
owner: human
change_policy: human-approval-required
---

# Document Governance

## 1. Purpose

この文書は、ソフトウェア開発における文書の権威、優先順位、参照関係、変更権限を定義する。

目的は、AIと人間が複数の文書を参照して開発するときに、

- どの文書を正本として扱うべきか
- 矛盾した情報のどちらを優先すべきか
- AIがどこまで文書を変更できるか

を機械的かつ一貫して判断できる状態を作ることである。

---

## 2. Terminology

### 2.1 正本（Canonical Source / Canonical Document）

「正本」は、もともと法律分野で用いられる用語である。

法律上の「正本」は、原本に基づいて作成され、一定の手続により原本と同一の内容または法的効力を持つものとして扱われる文書を指す。

ただし、このリポジトリでは法律上の厳密な意味をそのまま使用しない。

本ガバナンスにおける「正本」は、エンジニアリング上の **Canonical Source** または **Canonical Document** に相当し、次のように定義する。

> ある事項について、複数の情報が存在するときに、最終的な判断の基準として参照すべき、明示的に指定された権威ある情報源。

例えば、同じ仕様について、

```text
README
設計資料
実装メモ
過去のIssue
Product Contract
```

に異なる内容が書かれている場合、その事項について `Product Contract` が正本として指定されていれば、他の資料ではなく `Product Contract` を基準に判断する。

正本であることは、次を意味しない。

- 一度作ったら変更できない
- 内容が永久に固定される
- 文書だけが正本になれる
- Tierが高ければあらゆる事項の正本になる

正本も、定められた変更手続きを経て更新できる。

重要なのは、

```text
どこを変更してよいか
```

ではなく、

```text
その事項について、最終的にどこを参照するか
```

が明確になっていることである。

### 2.2 Source of Truthとの関係

「正本」と「Source of Truth」は近い概念として扱う。

ただし、Source of Truthは文書に限らない。

例えば、

```text
製品仕様
→ Tier 2 Product Contract

API仕様
→ 承認されたAPI Contract

現在の実装
→ Source Code

実行可能な振る舞い
→ Automated Tests

外部サービスの現在状態
→ 当該サービスの公式APIまたは管理画面
```

のように、情報の種類によって正本となる対象は異なる。

したがって、

> すべての情報について一つの巨大な正本を作る

ことは目的としない。

各ドメイン・各事項について、適切な正本を明確にする。

### 2.3 「正本」と「Tier」の違い

Tierは、

> 文書の権威の階層

を表す。

正本は、

> 特定の事項について最終的に参照すべき情報源

を表す。

そのため、Tier 1の文書だからといって、すべての情報について正本になるわけではない。

例えば、

```text
AIの変更権限
→ Tier 1 AI Agent Policy が正本

製品の振る舞い
→ Tier 2 Product Contract が正本

現在使われているクラス名
→ Source Code が正本
```

となり得る。

---

## 3. Tier Model

文書は4つのTierへ分類する。

```text
Tier 1: Constitution
Tier 2: Contract
Tier 3: Knowledge / Procedure
Tier 4: Derived View
```

数字が小さいTierほど高い権威を持つ。

```text
Tier 1
  ↓
Tier 2
  ↓
Tier 3
  ↓
Tier 4
```

---

## 4. Tier 1 — Constitution

Tier 1は、複数のプロジェクトへ共通して適用する統治原則である。

例:

- 文書ガバナンス
- AIエージェントの権限原則
- 変更管理原則
- セキュリティの基本原則
- 品質の基本原則

Tier 1は、個別プロジェクトの実装や一時的な技術選択へ依存してはならない。

Tier 1は最上位の文書であり、Tier 2、Tier 3、Tier 4を権威の根拠としてはならない。

---

## 5. Tier 2 — Contract

Tier 2は、個別プロジェクトにおいて、人間が承認した契約・制約・意思決定を表す。

例:

- Product Contract
- Release Policy
- Security Policy
- Architecture Constraints
- External Change Policy
- 承認済みArchitecture Decision Record

Tier 2は、実装方法そのものではなく、

> 実装が満たさなければならない条件

を定義する。

Tier 2は、Tier 3またはTier 4を権威の根拠としてはならない。

Tier 2はTier 1に従わなければならない。

---

## 6. Tier 3 — Knowledge / Procedure

Tier 3は、現在の設計、実装、運用方法を説明する文書である。

例:

- Architecture Guide
- Implementation Guide
- Runbook
- Setup Guide
- Troubleshooting Guide
- Code Reading Guide

Tier 3は、コードや外部サービスの変化に追従して更新される。

Tier 3はTier 1およびTier 2に従わなければならない。

Tier 3とTier 2が矛盾する場合、Tier 2を優先し、Tier 3を修正する。

---

## 7. Tier 4 — Derived View

Tier 4は、他の情報から生成または導出される補助資料である。

例:

- 文書インデックス
- コードマップ
- 依存関係グラフ
- 要約
- 自動生成された一覧
- AI向けコンテキスト
- Traceability Report

Tier 4は、仕様や契約を決定するための正本として扱ってはならない。

Tier 4が上位Tierまたは実装と矛盾する場合、Tier 4を再生成または修正する。

---

## 8. Reference Direction

文書が権威の根拠として参照できる方向を、次のように定める。

```text
Tier 1 → Tier 1

Tier 2 → Tier 1
         Tier 2

Tier 3 → Tier 1
         Tier 2
         Tier 3

Tier 4 → Tier 1
         Tier 2
         Tier 3
         Tier 4
```

上位Tierの文書は、下位Tierの文書を自らの正当性や契約内容の根拠としてはならない。

ただし、説明・例示・現状確認など、権威を委譲しない非規範的な参照は許容できる。

重要なのは、

> 下位Tierの記述によって、上位Tierの意味が決定されないこと

である。

---

## 9. Conflict Resolution

文書間に矛盾がある場合、原則として次の優先順位を使用する。

```text
Tier 1 > Tier 2 > Tier 3 > Tier 4
```

ただし、Tierが高いという理由だけで、その文書があらゆる事実の正本になるわけではない。

各情報には、それぞれ適切なSource of Truthを定義する。

例:

```text
開発統治原則
→ Tier 1

製品契約
→ Tier 2 Product Contract

API Contract
→ 承認されたAPI定義

現在の実装
→ Source Code

実行可能な振る舞いの証拠
→ Tests

生成済み一覧
→ Tier 4 Generated Document
```

「Tier」と「Source of Truth」は関連するが、同一概念ではない。

---

## 10. Canonical Documents

正本となる文書は、明示的にその権威を宣言する。

推奨するfrontmatter:

```yaml
---
doc_id: PROJECT-DOMAIN-001
tier: 2
status: approved
authority: canonical
owner: human
change_policy: proposal-only
---
```

`status: draft` の文書は、承認済みの正本として扱わない。

同じ事項について複数の正本を作ることは避ける。

---

## 11. AI Editability

基本ルールは次のとおりとする。

```text
Tier 1:
人間の承認なしに確定変更しない。

Tier 2:
通常の実装作業の一部として確定変更しない。
必要な場合は変更提案として扱う。

Tier 3:
上位Tierと矛盾しない範囲でAIが更新できる。

Tier 4:
再生成または自動更新できる。
```

AIの具体的な行動規則は `AI_AGENT_POLICY.md` で定義する。

---

## 12. Code and Documentation

コードを変更した結果、Tier 3が古くなった場合、コード変更と同じ作業の中でTier 3を更新することが望ましい。

一方、コード変更によってTier 2との矛盾が生じる場合、

Tier 2を実装へ合わせて自動的に書き換えてはならない。

この場合は、

```text
Tier 2
↓
Implementation
```

の契約違反として扱い、

- 実装をTier 2へ合わせる
- Tier 2変更を人間へ提案する

のいずれかを行う。

---

## 13. Public Information Principle

公開リポジトリに置かれるTier 1文書は、公開されること自体を前提として設計する。

Tier 1には、個別プロジェクトの秘密情報、内部構成、認証情報、個人情報を含めない。

公開の必要性がない情報は、Tier 1へ含めない。
