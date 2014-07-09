# GitHub練習

いろいろなやりかたがありますが、一例を挙げます。

## 追加、修正を行う場合の流れ

1. ローカルでmasterから新しいブランチを作成する。例えばb1というブランチを作成したとする。
2. ローカルのb1ブランチに修正をコミットする
3. GitHubにb1ブランチを作成し、そこにpushする
4. GitHub上で、b1からmasterにPullRequestを作成する
5. PullRequestを別の人がレビューする
6. 必要があればローカルのb1ブランチにさらに修正を行い、GitHubのb1ブランチにpushする
7. レビューが通ればPullRequestをマージする

コマンドラインで言うと以下。

```
# まずはmasterブランチに移動する
# その後、masterから新しいブランチを作成する
% git checkout master
% git branch b1

# b1ブランチに移動する
% git checkout b1

# 修正を行い、コミットする
edit
edit
edit
% git add -u
% git commit

# GitHubのb1ブランチにpushする
# b1ブランチにいることを確認してからpushすること
% git push origin b1

# PullRequestはGitHubのWeb画面で行う
# Switch branchesでb1ブランチに切り替えてPull Requestを選ぶ

# その後も必要があれば修正を行う
# PullRequestを作った後も同じブランチで修正を行って良い
edit
edit
% git add -u
% git commit
% git push origin b1
```

## 普通に必要になる操作

### リモートリポジトリのmasterをローカルリポジトリのmasterに反映させる

PullRequestをマージするとGitHubのリポジトリのmasterが進んでいく。
それをローカルに取り込むときはこうする。

```
# 方法1
% git checkout master
% git pull origin master

# 方法2
% git checkout master
% git fetch origin
% g merge origin/master
```

### PullRequestをマージ済のブランチがいらないので消したい

PullRequestをmasterにマージして、というのを繰り返すと、ローカルにマージ済ブランチがいっぱい溜まってくる。
それがうっとおしいと思うときはこうやって削除する。

```
# マージ済ブランチ一覧を表示する
% git branch --merged
# 削除する
% git branch -d 削除したいブランチ名
```

## 間違いから復旧する系の操作

### addを取り消したい

```
# addしたものが全部取り消されて、修正をまだaddしていない状態に戻る
% git reset HEAD

# 一部のファイルだけaddを取り消す場合
% git reset HEAD file
```


### コミットしていない修正を取り消したい

ローカルで修正を行っていたが、間違えていたので修正を取り消したいときはこうする。
svnのrevertに相当する操作。
これをすると、add済のものもそうでないものも含めて、コミットしていない変更がすべてなくなる。

```
% git checkout HEAD .
# または
% git reset --hard HEAD
```

### ブランチを切らずにmasterで作業したが、やっぱりブランチを作りたい

ブランチを切って作業したかったのに間違ってmasterにコミットしてしまったときはこうやって元に戻す。
本当はfeatureブランチを作ってそこにコミットしたかったのに、間違えてmasterにコミットしてしまったのを復旧させる例。

```
# 今masterにいて、まちがって1つコミットしてしまったとする
# 今のmasterの最新からブランチを作成する
% git branch feature
# この時点ではmasterとfeatureは一致している

# masterのコミットを1つ取り消す。
# これでmasterは1つ古くなる(1つコミットが減る)がfeatureはそのままなのでうまくいく
% git reset --hard 'HEAD^'
```

masterに複数のコミットをしてしまった場合はその回数分だけ`^`を重ねる。
例えば3回間違ってコミットした場合はこうする。

```
% git reset --hard 'HEAD^^^'
```

または`~n`を使っても良い。

```
% git reset 'HEAD~'

# git reset --hard 'HEAD^^^'と同じ
% git reset --hard 'HEAD~3'
```

### コミットするブランチを間違えた

コミットするブランチを間違えた場合はこうやって復旧する。
例えば、本当はfeature/1ブランチにコミットしたかったのに間違ってfeature/2ブランチにコミットしてしまった場合の復旧方法は以下。

```
# 今はfeature/2ブランチにいたとする
# feature/2のコミットを1つ取り消す
# --hardはつけていないので、作業コピーには修正は残っている
% git reset 'HEAD^'

# コミットしたいブランチに移動する
# まだコミットしていない修正はブランチを移動してもついてくる
% git checkout feature/2

# 後は普通にコミットする
% git add -u
% git commit
```


## 参考ページ

[Git初心者が絶対に覚えておくべきコマンド - idesaku blog](http://d.hatena.ne.jp/idesaku/20091106/1257507849)
[gitで一度行った変更をなかったことにする方法4つ - TIM Labs](http://labs.timedia.co.jp/2011/02/git-various-undo.html)
[gitでアレを元に戻す108の方法 - TIM Labs](http://labs.timedia.co.jp/2011/08/git-undo-999.html)

