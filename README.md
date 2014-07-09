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

