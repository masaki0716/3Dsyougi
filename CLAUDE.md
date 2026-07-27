# 3Dsyougi — CLAUDE.md

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
- D-3: 投げ銭ボタン（応援するリンク）追加（`claude/tip-jar`、PRマージ済み）
- B-3: 対局コードの満室通知・切断時専用画面・再生成ボタン追加（`claude/multiplayer-matchmaking`、push済み・PR作成待ち）
- B-4/B-5: three.js関連のvendor自前ホスティング、フォント埋め込みは見送りと判断（`claude/tech-polish`、push済み・PR作成待ち）
- worktree CLAUDE.mdの「ブランチスコープ節」がmainにマージされてしまう事故の再発と根本修正（下記「⚠️」参照）

次セッションでは以下のいずれかから着手：
- 残り2並行worktreeブランチ（multiplayer-matchmaking / tech-polish）のPRを作成・マージする（下記「⚠️」の手順でスコープ節を削除してから）
- フェーズC（GitHub Pagesへのデプロイ設定）に着手
- B-2（Cookie同意/CMP）の検討

**並行作業ブランチのindex.html競合リスクに備える**（下記「並行作業ブランチ」参照）: `multiplayer-matchmaking`・`tech-polish`の2ブランチが同じ`index.html`を編集している。**完了したブランチから順次PR→マージし、残りのブランチは都度 `git fetch origin && git merge origin/main` で追従する**運用にする（グローバルCLAUDE.mdルール16）。

**⚠️ worktreeのCLAUDE.mdに「## このブランチのスコープ」節を書いたら、PR作成前に必ず削除する**: 2026-07-27、2度目の事故が発生。各worktreeのCLAUDE.mdにブランチ固有の「## このブランチ（claude/xxx）のスコープ」節を追記する運用にしていたが、この節は**通常のPRマージで無条件にmainへ混入する**。1つ目のブランチ（tip-jar）のPRをマージした結果、mainのCLAUDE.md冒頭にtip-jar固有のスコープ節がそのまま残ってしまい、その状態で2つ目のブランチ（multiplayer-matchmaking）のPRを作ろうとしたところ、mainと当該ブランチがそれぞれ異なる「## このブランチのスコープ」節を持っていたためコンフリクトになった（この時点でmainを直接編集して該当節を削除し復旧、詳細は本セッションのgit履歴参照）。
  - 根本原因: ブランチ固有のスコープ節は「そのworktreeで作業する間だけ必要な情報」であり、mainには存在してはいけない内容なのに、通常のファイルとして共有ファイル（CLAUDE.md）に書いていたため、マージのたびにmainを汚染し、後続ブランチとの衝突を生んでいた。
  - **今後のルール**: worktreeでブランチ固有のスコープ節を書くのは作業中は構わないが、**PRを作成する直前に、そのブランチのCLAUDE.mdからスコープ節（見出しごと）を削除するコミットを追加する**。こうすればPRの差分にスコープ節が含まれず、mainを汚染しない。あるいは、そもそもスコープ節を`CLAUDE.md`本体に書かず、worktree直下に`.gitignore`対象の別ファイル（例: `SCOPE.local.md`）として書く運用に変えることも次回検討する。

## 並行作業ブランチ（worktree、2026-07-26作成）

`C:\Users\masas\worktrees\` 配下に、フェーズAの意思決定に依存しない範囲のタスクをworktreeとして切り出し済み。各worktreeのCLAUDE.mdにスコープが書いてある。

| worktreeパス | ブランチ | 担当タスク | index.htmlを触るか | 状態 |
|---|---|---|---|---|
| `C:\Users\masas\worktrees\3Dsyougi-multiplayer-matchmaking` | `claude/multiplayer-matchmaking` | B-3 対局コード仕様見直し(衝突/荒らし/切断対応) | 触る | **完了・PR作成待ち**（2026-07-27） |
| `C:\Users\masas\worktrees\3Dsyougi-tech-polish` | `claude/tech-polish` | B-4 CDN依存見直し + B-5 フォント依存対応(任意) | 触る | **完了・PR作成待ち**（2026-07-27） |
| `C:\Users\masas\worktrees\3Dsyougi-privacy-policy-draft` | `claude/privacy-policy-draft` | B-1 プライバシーポリシー下書き | 触らない(新規`privacy.html`のみ) | **完了、PRマージ済み** |
| `C:\Users\masas\worktrees\3Dsyougi-tip-jar` | `claude/tip-jar` | D-3 投げ銭ボタン組み込み(URLはプレースホルダ) | 触る | **完了、PRマージ済み** |

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
