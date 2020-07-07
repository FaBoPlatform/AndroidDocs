# Appendix7.Githubのコマンドをいじる

## RepoをClone

```
git clone https://github.com/akira-sasaki/GitTest.git
```

``` 
ls
.       ..      GitTest
```

```
cd GitTest
```

```
ls -a
.		..		.git		README.md
```

## TEST.mdを追加

TEST.mdを任意のエディタで追加する。

```
ls
README.md	TEST.md
```

## Addする

Currentフォルダのファイルをaddする。

```
git add .
```

Statusで確認する。

```
git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	new file:   TEST.md
```

new file: にTEST.mdが記載されていれば、addできている。

## Commitする

```
git commit -m "add TEST.md"
```

## Pushする

```
git push -u origin master
```



