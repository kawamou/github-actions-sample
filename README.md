# github-actions-sample!

### 概要
- Github同梱のCI/CDツール
- デフォルトで全リポジトリでGitHub Actionsは有効
- Github Actionsの起動方法は3つ
    - リポジトリで起きたことをトリガーに処理を走らせる: issues, comments, push, PR
    - cron: cronと同じように特定の時間に動作させられる
    - REST API: curlとかrest叩けるやつで叩ける
- 裏側はAzureのインスタンス
    - ipレンジは公開されているがあくまでazureのipレンジなのでip制限には不向き
    - ip制限ではなくセルフホスティング使おう
- JS(node12)で実行するかDockerで実行するか選べる

### 制限(2020.06時点)
- リポジトリごとに最大で20ワークフローまで同時実行可
- リポジトリ内のすべてのアクション全体で1時間につき1,000APIリクエスト
- ワークフロー内のすべてのジョブの最大実行時間は6時間まで
- リポジトリごとにすべてのワークフロー全体で20ジョブまで同時実行できる

### 概念
- 全体の流れであるワークフローに, 処理(ジョブ)を記述する. ワークフローはymlに記載されたトリガーを契機に走り出す
- ワークフロー: 複数のジョブで構成. ひとつのリポジトリに複数のWFの設定可能
- ジョブ: 複数のステップで構成. 各ステップはコマンドの実行か(echoとかcatとか)アクション(コードで書いたりマケプレに転がってるやつ)等で構成
    - 何も指定しないとジョブは並列実行
- アクション
    - 公開するアクションの場合, アクションのためのリポジトリを作成して, 他のコードとは別に管理することが推奨
    - 公開しない場合はどこに保存してもよく, 単独のリポジトリからのみ利用する場合は, .github下に保存することが推奨

### 使い方
- CI/CDを実施したいリポジトリ直下の`.github/workflows/`以下にymlを置く
- ymlがpushされた瞬間からCI/CDが動き出す
- 実行結果はリポジトリのActionsタブから確認可能
    - 本リポジトリだと, masterにpushされると`Actions > All workflows > commit名 > build >  Run a one-line script`に`Hello, world!`が表示される