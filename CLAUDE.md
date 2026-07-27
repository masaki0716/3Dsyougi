# 3Dsyougi — CLAUDE.md

## このブランチ（claude/multiplayer-matchmaking）のスコープ（最優先で読む）

`公開準備タスクリスト.md` の **B-3: 対局コード仕様の見直し** を担当するworktree。

- 現状「身内でコードを共有」前提の実装（`index.html:759`付近、5文字の対局コードに3人目が来たら無視＝早い者勝ち）を、不特定多数が使う公開ゲーム向けに見直す
  - **コード衝突対策**: 同じ5文字コードを別の意図の組が使ってしまう確率・対処（コード長を伸ばす／再生成導線を用意する等）
  - **荒らし対策**: 無関係な相手が乱入・妨害してくる動きへの対処
  - **切断対応**: 対局途中の切断時のUI・再接続導線（現状の挙動を確認した上で改善要否を判断）

**スコープの境界**:
- フェーズA（意思決定: IPリスク方針・収益化手段・ホスティング先・ドメイン方針）には依存しない範囲で完結させる。広告タグ・収益化導線・本番URL固有の設定はここでは行わない。
- Trystero（P2P・Nostrシグナリング）の基本的な通信方式自体は変更しない。マッチングロジック（コード生成・参加制御）の改善が主眼。
- 単一HTML(`index.html`)構成を維持する。ビルドツール導入はしない。

**動作確認について**:
- テストには`起動と終了手順.md`の手順（HTTPS配信必須）が必要。
- `_cert/`（自己署名証明書）は`.gitignore`対象のためこのworktreeには存在しない。本体(`C:\Users\masas\knowledgeBaseSystem\3Dsyougi\_cert`)からコピーするか、手順書に従って作り直すこと。
- 同一PC上の2ブラウザタブ（またはPC+スマホ）で同じHTTPS URLにアクセスして確認する。

**完了時**: `公開準備タスクリスト.md`のB-3にチェックを入れるPRを作成する。他ブランチとは独立にPR化・マージしてよい。

---

## プロジェクト概要

立体将棋（9×9×9）— 単一HTML(`index.html`)のThree.jsゲーム。ふざけ要素ありの個人開発。
knowledgeBaseSystemベースディレクトリの管理下プロジェクト一覧（ルートCLAUDE.md）には未記載の新規プロジェクト。

## 現在のフェーズ

**フェーズ0: 実験的デプロイ・収益化検証**

「公開してアフィリエイト/スマホアプリ広告で収入を得られるか」を検証する実験プロジェクトと位置づける。ゲーム内容の完成度追求より、デプロイ〜収益導線の検証を優先する。

## 次セッションで最初にやること

**2026-07-27セッションで完了した内容**（詳細は`公開準備タスクリスト.md`）:
- フェーズA（A-1〜A-4）の意思決定
- A-1実装: 「ザ・ワールド！」→「勝利！！」、BOOM_WORDSの「メメタア」削除（`claude/ip-risk-wording`、PRマージ済み）
- B-1: `privacy.html`（日英併記の下書き）追加（`claude/privacy-policy-draft`、PRマージ済み）
- worktree運用の事故修正（下記「⚠️」参照）と4並行ブランチのmain追従

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
| `C:\Users\masas\worktrees\3Dsyougi-multiplayer-matchmaking` | `claude/multiplayer-matchmaking` | B-3 対局コード仕様見直し(衝突/荒らし/切断対応) | 触る | **完了・PR作成待ち**（2026-07-27） |
| `C:\Users\masas\worktrees\3Dsyougi-tech-polish` | `claude/tech-polish` | B-4 CDN依存見直し + B-5 フォント依存対応(任意) | 触る | 未着手・main追従済み（2026-07-27時点） |
| `C:\Users\masas\worktrees\3Dsyougi-privacy-policy-draft` | `claude/privacy-policy-draft` | B-1 プライバシーポリシー下書き | 触らない(新規`privacy.html`のみ) | **完了、PRマージ済み** |
| `C:\Users\masas\worktrees\3Dsyougi-tip-jar` | `claude/tip-jar` | D-3 投げ銭ボタン組み込み(URLはプレースホルダ) | 触る | **完了・PR作成待ち**（2026-07-27） |

**注意**: いずれの並行ブランチもA-1〜A-4の意思決定には依存しないスコープに限定している。各ブランチで作業する際は、そのworktree直下の`CLAUDE.md`のスコープ説明を読むこと。オンライン対戦のテストが必要な場合、`_cert/`は`.gitignore`対象のため各worktreeには存在せず、本体からコピーするか作り直す必要がある（`multiplayer-matchmaking`のCLAUDE.mdに記載済み）。

## 技術スタック

| 用途 | 技術 |
|---|---|
| 3D描画 | Three.js r128（CDN: cdnjs / jsdelivr） |
| オンライン対戦 | Trystero（P2P、既定Nostrシグナリング、`esm.run`から動的import） |
| ホスティング | GitHub Pages（無料サブドメイン `masaki0716.github.io/3Dsyougi` で開始、2026-07-27決定・未デプロイ） |
| 収益化 | AdSense + 投げ銭 + itch.io 掲載（2026-07-27決定、未着手） |

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
