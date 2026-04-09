# Claude Code テンプレート候補 調査レポート
調査日: 2026-04-09

## 調査ソース
- GitHub: awesome-claude-code, awesome-claude-skills, awesome-claude-code-toolkit, claude-code-workflows, tdd-guard, claude-format-hook, claude-code-django 等
- Web: Claude Code公式ドキュメント, DEV Community, Medium, builder.io, eesel.ai, morphllm.com, DataCamp, uxplanet.org 等
- Reddit: r/ClaudeAI, r/ClaudeCode (週間4,200+投稿者)

## 候補一覧

### 1. TDD ワークフロー スキル
- カテゴリ: quality
- 優先度: 高
- ニーズの根拠: TDD専用スキル/フックの需要が非常に高い。GitHub上にtdd-guard(https://github.com/nizos/tdd-guard)やclaude-code-workflowsのTDDモジュール(https://github.com/shinpr/claude-code-workflows)が登場。alexop.devでは「Forcing Claude Code to TDD: An Agentic Red-Green-Refactor Loop」(https://alexop.dev/posts/custom-tdd-workflow-claude-code-vue/)という記事が話題に。aihero.devでも「My Skill Makes Claude Code GREAT At TDD」(https://www.aihero.dev/skill-test-driven-development-claude-code)として紹介。
- 概要: Red-Green-Refactorサイクルを強制するSKILL.mdテンプレート。テストを先に書き、実装コードを後から書くフェーズゲートを定義。

### 2. 自動フォーマット フック (Prettier/ESLint/Biome)
- カテゴリ: automation
- 優先度: 高
- ニーズの根拠: PostToolUseフックによる自動フォーマットは最も需要の高いフック設定の一つ。GitHub上にclaude-format-hook(https://github.com/ryanlewis/claude-format-hook)が公開され、JS/TS/Python/Go/Kotlin/Markdownに対応。DEV Community, DataCamp, morphllm.com等で多数の解説記事あり。
- 概要: Claude Codeがファイルを編集するたびにPrettier/ESLint/Biome等のフォーマッターを自動実行するPostToolUseフック設定テンプレート。

### 3. Git コミット規約フック (Conventional Commits)
- カテゴリ: quality
- 優先度: 高
- ニーズの根拠: Conventional Commits(feat:, fix:, docs:等)の強制はチーム開発で頻繁に求められる。GitHub Issuesでもフック関連のFeature Requestが活発(https://github.com/anthropics/claude-code/issues/4834)。reading.sh, markaicode.com等で具体的なフック設定ガイドが公開されている。
- 概要: ClaudeのコミットメッセージをConventional Commits形式に強制するPreToolUseフックとCLAUDE.md指示のテンプレート。

### 4. Python/FastAPI CLAUDE.md テンプレート
- カテゴリ: stack
- 優先度: 高
- ニーズの根拠: Python系フレームワーク(FastAPI/Django/Flask)向けCLAUDE.mdの需要が非常に高い。DEV Communityで「Python/FastAPI Development with Claude Code」(https://dev.to/myougatheaxo/pythonfastapi-development-with-claude-code-claudemd-setup-hooks-and-best-practices-1f11)が人気。awesome-claude-code-toolkitにもpython-project.mdテンプレートあり(https://github.com/rohitg00/awesome-claude-code-toolkit/blob/main/templates/claude-md/python-project.md)。clauderules.netでもPython向けルールが提供されている。
- 概要: FastAPI/Django向けのCLAUDE.md。型注釈必須、Pydantic v2バリデーション、async/awaitパターン、pytest設定等を含む。

### 5. マルチエージェント / Worktree 設定テンプレート
- カテゴリ: automation
- 優先度: 高
- ニーズの根拠: マルチエージェントパターンはRedditで最もアクティブに議論されているトピック。shipyard.build, claudelab.net, morphllm.com等で詳細ガイドが公開。GitHub上にもwshobson/agents(https://github.com/wshobson/agents)やclaude-code-ultimate-guide等のリポジトリあり。
- 概要: Git worktreeを使ったサブエージェントの分離実行設定。リファクタリング・テスト生成・大規模変更を並列実行するためのサブエージェント定義テンプレート。

### 6. GitHub Actions CI/CD レビュー設定
- カテゴリ: automation
- 優先度: 高
- ニーズの根拠: Anthropic公式のclaude-code-action(https://github.com/anthropics/claude-code-action)がリリースされ、GitHub Marketplaceでも公開中。PRの自動レビュー、セキュリティチェック、Issue自動トリアージ等が実現可能。DEV Community, markaicode.com, systemprompt.io等で多数の設定ガイドあり。Derivでは自動セキュリティレビューの実践事例が公開(https://derivai.substack.com/p/automated-security-code-reviews-claude-code-github-actions)。
- 概要: Claude Code Action を使ったPR自動レビュー、Issue対応のGitHub Actionsワークフロー設定テンプレート。

### 7. モノレポ用 階層型 CLAUDE.md テンプレート
- カテゴリ: team
- 優先度: 中
- ニーズの根拠: モノレポでのCLAUDE.md管理は頻出の課題。DEV Communityで「How I Organized My CLAUDE.md in a Monorepo」(https://dev.to/anvodev/how-i-organized-my-claudemd-in-a-monorepo-with-too-many-contexts-37k7)が話題に。Mediumでは「Virtual Monorepo Pattern」(https://medium.com/devops-ai/the-virtual-monorepo-pattern-how-i-gave-claude-code-full-system-context-across-35-repos-43b310c97db8)として35リポジトリ横断の事例も。Skoolコミュニティでも実用的な質問が活発。
- 概要: ルートCLAUDE.mdから@importで各パッケージのCLAUDE.mdを参照する階層構造テンプレート。コンテキスト肥大化を防ぐ設計パターン。

### 8. Docker サンドボックス設定テンプレート
- カテゴリ: quality
- 優先度: 中
- ニーズの根拠: セキュリティ意識の高いユーザーからの需要が増加。Docker公式ブログ(https://www.docker.com/blog/docker-sandboxes-run-claude-code-and-other-coding-agents-unsupervised-but-safely/)やtruefoundry.com, mintmcp.com等で詳細ガイドが公開。2026年3月にはサンドボックスバイパスの脆弱性が報告され、防御の重要性が再認識された。
- 概要: Claude CodeをDocker/microVMで安全に実行するためのサンドボックス設定テンプレート。ファイルシステム分離、ネットワーク制限を含む。

### 9. コスト最適化 CLAUDE.md テンプレート
- カテゴリ: quality
- 優先度: 中
- ニーズの根拠: トークン使用量とコストは全ユーザー共通の関心事。GitHub上にclaude-token-efficient(https://github.com/drona23/claude-token-efficient)が公開され、出力の冗長さを抑制するCLAUDE.mdが提供されている。mindstudio.ai, claudefa.st, systemprompt.io等でコスト削減ガイドが多数公開。平均$6/日だが40-70%削減可能との情報。
- 概要: トークン消費を最小化するCLAUDE.md設定テンプレート。簡潔な出力指示、/clearの推奨タイミング、Sonnet/Opus使い分けガイドラインを含む。

### 10. ドキュメント自動生成スキル
- カテゴリ: automation
- 優先度: 中
- ニーズの根拠: levnikolaevich/claude-code-skills(https://github.com/levnikolaevich/claude-code-skills)でdocumentation generation機能が提供されている。Mediumの「10 Must-Have Skills」(https://medium.com/@unicodeveloper/10-must-have-skills-for-claude-and-any-coding-agent-in-2026-b5451b013051)でもproject-docsスキルが推奨されている。
- 概要: コードベース分析、APIドキュメント生成、アーキテクチャ図作成を行うSKILL.mdテンプレート。

### 11. セキュリティレビュー スキル
- カテゴリ: quality
- 優先度: 中
- ニーズの根拠: Derivの事例(https://derivai.substack.com/p/automated-security-code-reviews-claude-code-github-actions)でセキュリティ特化のコードレビューが実践されている。OWASP Top 10チェック、依存関係脆弱性スキャン等のニーズがRedditでも頻出。
- 概要: OWASP Top 10準拠のセキュリティチェックリストを適用するSKILL.md。SQLインジェクション、XSS、認証不備等を自動検出。

### 12. 自己検証 Stop フック (Work Verification)
- カテゴリ: quality
- 優先度: 中
- ニーズの根拠: Claude Codeが応答完了前に作業の完了度を自己検証するStop hookパターンが注目されている。公式ドキュメント(https://code.claude.com/docs/en/hooks-guide)でpromptフックとの組み合わせ事例が紹介。eesel.ai, DataCamp等でも解説記事あり。テスト実行忘れや不完全な実装を防ぐ効果がある。
- 概要: Claudeが応答を停止する前に、要求されたタスクが完了しているかテストが実行されたかを検証するStopフック設定テンプレート。

### 13. 機密ファイル保護フック
- カテゴリ: quality
- 優先度: 低
- ニーズの根拠: .env, package-lock.json, .git/等の機密ファイルの誤編集防止は基本的なニーズ。公式ドキュメント(https://code.claude.com/docs/en/hooks-guide)でPreToolUseフックの代表例として紹介。既存のsafety-hooksと一部重複するが、より詳細なファイルパターンマッチングに特化。
- 概要: 機密ファイル・ロックファイルへの書き込みをブロックするPreToolUseフック。.env, *.pem, *.key, lock files等のパターンマッチング設定。

### 14. セッション初期化フック (SessionStart)
- カテゴリ: automation
- 優先度: 低
- ニーズの根拠: SessionStartフックでgit statusやdocker compose状態を自動取得するパターンが複数のブログで紹介されている。cameronwestland.com(https://cameronwestland.com/building-my-first-claude-code-hooks-automating-the-workflow-i-actually-want/)等で実践例あり。
- 概要: セッション開始時にプロジェクト状態（git status、テスト結果、依存関係チェック等）を自動取得するSessionStartフック設定テンプレート。

## 優先度サマリー

| 優先度 | テンプレート数 | 主な候補 |
|--------|--------------|---------|
| 高 | 6件 | TDD, 自動フォーマット, コミット規約, Python/FastAPI, マルチエージェント, GitHub Actions |
| 中 | 6件 | モノレポ, Docker, コスト最適化, ドキュメント生成, セキュリティレビュー, 自己検証 |
| 低 | 2件 | 機密ファイル保護, セッション初期化 |

## 備考
- 既存テンプレート(memory-system, global-claude-md, notification-hook, daily-report, safety-hooks, nextjs-claude-md, shared-claude-md, code-review-skill)との重複を避けて選定
- 機密ファイル保護フック(#13)は既存safety-hooksと類似するが、より詳細なパターンマッチングに特化した差別化が可能
- TDDワークフローと自動フォーマットフックは、コミュニティでの言及頻度と実装例の多さから最優先で取り組むべき
