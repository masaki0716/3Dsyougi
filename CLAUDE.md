# 3Dsyougi — CLAUDE.md（ブランチ: claude/tech-polish）

## ⚠️ このブランチのスコープ（最優先で読む）

`公開準備タスクリスト.md` の **B-4: CDN依存の見直し** と **B-5: フォント依存の扱い**（両方とも「任意」項目）を担当するworktree。

- **B-4 CDN依存の見直し**（`公開・収益化リスク検討.md` A）
  - 現状 three.js@cdnjs、jsdelivr、Trystero(`esm.run`から動的import) と3種のCDNに依存。いずれも無料・商用利用可だが単一障害点でSLAなし
  - 自前バンドル配信（ローカルにライブラリを同梱する等）への切替が単一HTML構成を崩さずにできるか検討し、可能なら実施。難しければ「現状維持（許容）」の判断根拠を`公開・収益化リスク検討.md`に追記する
- **B-5 フォント依存の扱い**
  - 筆文字フォントがシステムフォント依存（Yu Mincho等）で、Windows以外はMSゴシック/汎用serifにフォールバックする
  - Webフォントの埋め込み（`@font-face`＋ライセンス確認）で解消するか、単一HTMLの軽量さを優先して許容するかを検討し、対応する場合は実装する

**スコープの境界**:
- フェーズA（意思決定）には依存しない。ゲームロジック・オンライン対戦仕様には触れない（`claude/multiplayer-matchmaking`ブランチの担当）。
- 単一HTML(`index.html`)構成を維持する。ビルドツール導入はしない（フォントを`@font-face`で埋め込む場合もBase64等でHTML内に収める）。
- どちらも「任意」項目のため、調査した上で「対応不要」と判断した場合はその判断根拠を記録するだけでもよい（無理に実装しない）。

**完了時**: `公開準備タスクリスト.md`のB-4・B-5にチェックを入れ、判断根拠を`公開・収益化リスク検討.md`に追記するPRを作成する。

---

## プロジェクト概要（プロジェクト全体の背景。以下は本体と共通）

立体将棋（9×9×9）— 単一HTML(`index.html`)のThree.jsゲーム。ふざけ要素ありの個人開発。
knowledgeBaseSystemベースディレクトリの管理下プロジェクト一覧（ルートCLAUDE.md）には未記載の新規プロジェクト。

## 現在のフェーズ

**フェーズ0: 実験的デプロイ・収益化検証**

「公開してアフィリエイト/スマホアプリ広告で収入を得られるか」を検証する実験プロジェクトと位置づける。ゲーム内容の完成度追求より、デプロイ〜収益導線の検証を優先する。

## 次セッションで最初にやること

**フェーズA（意思決定）・A-1実装・B-1（プライバシーポリシー下書き）は2026-07-27にすべて完了しmainにマージ済み**：

1. A-1: 「ザ・ワールド！」→**「勝利！！」**、BOOM_WORDSの「メメタア」→**削除**（`claude/ip-risk-wording`、PRマージ済み）
2. A-2: 収益化手段は **1(AdSense)+3(投げ銭)+5(itch.io)** の組み合わせで正式決定
3. A-3: ホスティング先は **GitHub Pages** に決定
4. A-4: ドメインは **無料サブドメイン（masaki0716.github.io/3Dsyougi）で開始**
5. B-1: `privacy.html`（日英併記の下書き）を追加（`claude/privacy-policy-draft`、PRマージ済み）

次セッションでは以下のいずれかから着手：
- 残り3並行worktreeブランチ（multiplayer-matchmaking / tech-polish / tip-jar）のいずれかで実装を進める
- フェーズC（GitHub Pagesへのデプロイ設定）に着手
- B-2（Cookie同意/CMP）の検討

**並行作業ブランチのindex.html競合リスクに備える**（下記「並行作業ブランチ」参照）: `multiplayer-matchmaking`・`tech-polish`・`tip-jar`の3ブランチが同じ`index.html`を編集する見込み。全部を並行フルスピードで進めて最後にまとめてマージすると衝突が大きくなりやすいため、**完了したブランチから順次PR→マージし、残りのブランチは都度 `git fetch origin && git merge origin/main` で追従する**運用にする（グローバルCLAUDE.mdルール16）。この3ブランチは2026-07-27時点でmain（フェーズA決定・A-1実装・B-1完了込み）に追従済みだが、次にどれかのPRをマージしたら、残りは再度 `git fetch origin && git merge origin/main` で追従すること。

**⚠️ worktreeブランチのCLAUDE.mdはheader/タイトル行を書き換えない**: 2026-07-27、`claude/privacy-policy-draft`のCLAUDE.md冒頭に付けていた「⚠️ このブランチのスコープ」見出し（ファイル先頭の`# 3Dsyougi — CLAUDE.md（ブランチ: ...）`という独自タイトル）が、PRマージ時にmain側の本来のタイトル・概要セクションを上書きしてしまい、mainマージ後に気づいて手動修正する事態になった。今後worktreeでブランチ固有のスコープを書く場合、**ファイル冒頭のタイトル行・共通セクションの構成は変えず**、スコープ説明は本文中の独立した見出し（例: `## このブランチのスコープ`）として追記するに留める。

## 並行作業ブランチ（worktree、2026-07-26作成）

`C:\Users\masas\worktrees\` 配下に、フェーズAの意思決定に依存しない範囲のタスクをworktreeとして切り出し済み。各worktreeのCLAUDE.mdにスコープが書いてある。

| worktreeパス | ブランチ | 担当タスク | index.htmlを触るか | 状態 |
|---|---|---|---|---|
| `C:\Users\masas\worktrees\3Dsyougi-multiplayer-matchmaking` | `claude/multiplayer-matchmaking` | B-3 対局コード仕様見直し(衝突/荒らし/切断対応) | 触る | 未着手・main追従済み（2026-07-27時点） |
| `C:\Users\masas\worktrees\3Dsyougi-tech-polish` | `claude/tech-polish` | B-4 CDN依存見直し + B-5 フォント依存対応(任意) | 触る | 未着手・main追従済み（2026-07-27時点） |
| `C:\Users\masas\worktrees\3Dsyougi-privacy-policy-draft` | `claude/privacy-policy-draft` | B-1 プライバシーポリシー下書き | 触らない(新規`privacy.html`のみ) | **完了、PRマージ済み** |
| `C:\Users\masas\worktrees\3Dsyougi-tip-jar` | `claude/tip-jar` | D-3 投げ銭ボタン組み込み(URLはプレースホルダ) | 触る | 未着手・main追従済み（2026-07-27時点） |

**注意**: いずれの並行ブランチもA-1〜A-4の意思決定には依存しないスコープに限定している。各ブランチで作業する際は、そのworktree直下の`CLAUDE.md`のスコープ説明を読むこと。オンライン対戦のテストが必要な場合、`_cert/`は`.gitignore`対象のため各worktreeには存在せず、本体からコピーするか作り直す必要がある（`multiplayer-matchmaking`のCLAUDE.mdに記載済み）。

## 技術スタック

| 用途 | 技術 |
|---|---|
| 3D描画 | Three.js r128（CDN: cdnjs / jsdelivr） |
| オンライン対戦 | Trystero（P2P、既定Nostrシグナリング、`esm.run`から動的import） |
| ホスティング | 未定（GitHub Pages / Cloudflare Pages 等を比較検討中） |
| 収益化 | 未定（AdSense / アフィリエイト / 将来的にAdMob） |

## 設計の絶対ルール

- 実装は単一HTML(`index.html`)を維持する範囲で行う。ビルドツール導入は収益化検証フェーズでは行わない（身軽さを優先）
- オンライン対戦の「選択マスのハイライトは相手に同期しない」という既存仕様を壊さない
- `_cert/` 配下の秘密鍵は絶対にコミットしない（git整備時に `.gitignore` 必須）
- 広告タグ・アフィリエイトリンクなど収益化に関わる変更を行う前に、著作権/商標リスク（BOOM_WORDS等）の対応状況を確認する

## セッション開始時に読むファイル

1. `CLAUDE.md`（本ファイル）
2. `公開準備タスクリスト.md`（やるべきことの全体像・進捗）
3. `公開・収益化リスク検討.md`
4. `収益化手段比較.md`
5. `起動と終了手順.md`
6. 必要に応じて `index.html` の該当箇所

## git運用

gitリポジトリは初期化済み（GitHub: `masaki0716/3Dsyougi`）。グローバルCLAUDE.mdの方針（フィーチャーブランチ運用、実装はpushまで、PR作成/マージはユーザー）に従う。

## セッション終了時にやること

- 進捗・意思決定を本ファイルの「現在のフェーズ」「次セッションで最初にやること」、および `公開準備タスクリスト.md` のチェック状態に反映する
- 新しい知見は `公開・収益化リスク検討.md`・`収益化手段比較.md`・本ファイルのいずれかに追記する
