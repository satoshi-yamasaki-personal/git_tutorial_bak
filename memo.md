# gitの初期設定(global)
## ファイルのありか
> ~/.gitconfig

## 各種値の設定
- username, email, editorを設定
> git config --global user.name ["user-name"]
> git config --global user.email [email]
> git config --global core.editor [code --wait]
> ※ VSCodeを設定

## 設定値の確認
> git config --list
> git config [component]

# gitの操作の流れ
- git add: ワークツリー -> ステージ
  ※ repositoryに圧縮ファイル、stageにインデックス(圧縮ファイルと実際のファイルのマッピング)が生成
- git commit: ステージ -> リポジトリ(この時点のスナップショットを都度保管)
  ※ repositoryにツリー(ファイル構成)とコミット(誰がいつコミットしたかの履歴)を作成(2回目以降はparentを保持)

## ローカルリポジトリの新規作成
> git init
-> .gitディレクトリ(=ローカルリポジトリ)が作成され、リポジトリ、圧縮ファイル、ツリーファイル、コミットファイル、インデックスファイル、設定ファイルetc...が生成

## GitHub上のプロジェクトから作成
> git clone [repository-url]

## 変更をステージに追加(add)
- git add [file | directory | .]
  ※'.'の場合は全て追加

## 変更をリポジトリに追加(commit)
- git commit [-m | -v]
  ※ -m: メッセージ付きで記録
    -v: エディタが起動し、変更内容を確認できる(事前に確認したい場合に使う。割とよく使う。)
    コメントは1行で書く or 1行目に要約/2行目を空白/3行目に理由を書く

## 差分を確認
- git status: ワークツリー、ステージ、リポジトリ間の差分ファイルを表示
- git diff [file]: git addする前の変更差分を表示
- git diff --staged: git addした後の変更差分を表示

## 変更履歴を確認
- git log: 変更履歴を表示
- git log --oneline: 1行で表示
- git log -p [file]: 特定ファイルの変更差分を表示
- git log -n [num]: コミット数を制限して表示

## ファイルの削除を記録
- git rm [file]: ファイルごと削除
- git rm -r [dir]: ディレクトリごと削除
- git rm --cached [file]: ファイルを残して削除履歴だけ記録(パスワードが入ったファイルとか)

## 削除ファイルの復活を記録
- git reset HEAD [file]: 
  ※ リポジトリからのみ削除の場合はこれだけで復活
- git checkout [file]: 

## ファイルの移動を記録
- git mv [old_file] [new_file]: ファイル名変更(ステージに記録)

## GitHubにプッシュ
- git remote add origin https://github.com/[user]/[repository].git: originという名前でリモートリポジトリ(GitHub)を新規追加
  ※ originがない場合、毎回URLを指定する必要がある。originで指定しておくとgit cloneの時にこのリポジトリを取得する
- git push [remote] [branch]: ローカルリポジトリの内容をリモートリポジトリに反映
  ※ git push origin masterみたいな感じ
    初回はgit push -u origin masterとやっておくと、次回以降の入力が楽になる
- 事前準備としてAccess Token生成が必要
  ※ User > Setttings > Developer settings > Personal access tokens
- リポジトリはGUIから作成しておく
  ※ User > Your Profile > Repositories > New

## コマンドにエイリアスをつける
※ 下記はあくまでサンプルなので、実際の付け方は自由
- git config --global alias.ci commit
- git config --global alias.st status
- git config --global alias.br branch
- git config --global alias.co checkout
  ※ --globalは~/.gitconfigを更新、ない場合はproject/.git/configを更新

## 管理しないファイルをGitの管理から外す
- 自動生成されるファイルやパスワードなどが入ったファイルは除外すべき
- .gitignoreファイルを作成して記載

## ファイルへの変更を取り消す(ワークツリーのファイルを元の状態に戻す)
- git checkout -- [file | directory | .]
  ※ '--'はブランチ名とファイル名が被った場合に明示的にファイルなどを指定するために入力
    実態としてはstageの状況を取得してワークツリーに反映
