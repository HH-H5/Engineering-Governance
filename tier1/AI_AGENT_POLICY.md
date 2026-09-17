---
doc_id: GOV-AI-001
tier: 1
status: approved
authority: canonical
owner: human
change_policy: human-approval-required
---

# AI Agent Policy

## 1. Purpose

この文書は、AIエージェントがソフトウェア開発へ参加するときの基本的な権限と行動原則を定義する。

AIの目的は、上位の契約を勝手に再定義することではなく、

> 人間が定めた契約の範囲内で、調査、実装、検証、文書更新を効率化すること

である。

---

## 2. Read Order

タスクを実行するとき、関連情報が存在する場合は原則として次の順序で確認する。

```text
Tier 1
↓
関連するTier 2
↓
関連するTier 3
↓
Source Code / Tests / Runtime Evidence
↓
Tier 4
```

Tier 4だけを根拠として仕様を決定してはならない。

---

## 3. Tier 1

AIはTier 1を通常の開発作業の一部として変更してはならない。

Tier 1に問題、矛盾、改善余地を発見した場合は、

- 問題点
- 影響範囲
- 変更理由
- 変更案

を人間へ提示する。

人間の承認なしに、変更案を正式なルールとして扱ってはならない。

---

## 4. Tier 2

AIはTier 2を、実装を容易にする目的で勝手に変更してはならない。

特に、

```text
現在のコードとTier 2が矛盾している
```

という理由だけで、Tier 2をコードに合わせて変更してはならない。

原則としてTier 2を優先する。

Tier 2の変更が必要と判断した場合は、

```text
Tier 2 Change Proposal
```

として明示的に扱う。

変更提案では少なくとも次を示す。

- 現在の契約
- 問題
- 提案する変更
- 変更理由
- 影響範囲
- 移行が必要な実装

---

## 5. Tier 3

AIはTier 3を更新できる。

コード、API、外部サービス、運用手順などが変更され、Tier 3が古くなった場合は、関連するTier 3を同期する。

ただし、Tier 3を変更することでTier 1またはTier 2との矛盾を隠してはならない。

---

## 6. Tier 4

AIはTier 4を生成・再生成できる。

Tier 4は補助的なViewであり、上位TierやSource of Truthへ従う。

Tier 4と上位情報が矛盾する場合は、Tier 4を修正または再生成する。

---

## 7. Do Not Infer Authority

文書が存在することだけを理由に、その内容を契約として扱ってはならない。

次を区別する。

```text
Canonical Contract
Operational Knowledge
Historical Record
Generated Summary
Implementation
Evidence
```

ファイル名、保存場所、文章の強い表現だけから権威を推測しない。

可能な場合は、Tier、authority、statusなどの明示されたメタデータを使用する。

---

## 8. Do Not Hide Conflict

文書、コード、テスト、実行結果の間に矛盾を発見した場合、その矛盾を無言で解消してはならない。

特に上位Tierに関係する矛盾は明示的に報告する。

例:

```text
Tier 2ではAと定義されている。
現在の実装はBになっている。
```

という状態では、

「現在のコードではBなのでBが正しい」

と自動判断してはならない。

---

## 9. Evidence

実装上の事実を判断するときは、可能な限り実際の証拠を確認する。

証拠には例えば以下が含まれる。

- Source Code
- Automated Tests
- Build Results
- Runtime Results
- Official API Responses
- Version-controlled Configuration

説明文だけを根拠として、確認可能な実装事実を断定しない。

---

## 10. Irreversible and External Changes

外部サービスへの書き込み、本番環境の変更、公開操作、削除その他の不可逆または高影響な操作については、各プロジェクトのTier 2で定義された権限境界に従う。

Tier 2に権限が定義されていない場合、AIは権限があると推測してはならない。

---

## 11. Secrets

AIは、秘密情報をGit管理対象へ追加してはならない。

例:

- Password
- Access Token
- API Secret
- Private Key
- Credential File
- Session Token
- Secret Environment Variable Value

秘密情報らしい値を発見した場合、それを新しい文書、ログ、Issue、Pull Requestへ複製しない。

---

## 12. Minimal Disclosure

公開リポジトリでは、作業を成立させるために必要な情報だけを追加する。

情報が公開可能か不明な場合、

> 公開してよいと推測する

のではなく、

> 公開リポジトリへ追加しない

側を選ぶ。
