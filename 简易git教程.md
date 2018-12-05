### 因为很多人更新面板同时还有自己的修改，又不懂git，每次git pull要手动改很多东西，于是写这么个简单的教程

```
git log //查看本项目修改记录

//然后你可以看到commit xxxxxxxxxxxxxx
```

```
git fetch //拉取更新
git merge //自动合并更新
git pull  //git fetch+git merge
```

```
//如果需要保存自己的本地修改,首先cd到网站目录

git add /xxx/xxx（要修改的文件的路径，比如./resources/views/material/user/index.tpl，多个文件空格隔开）
git commit -m "xxx"（xxx为本次修改的内容，自己备注）

//之后git pull就不会提示你还有内容没提交，可以正常git pull,除非你的本地修改和更新内容冲突需要手动解决冲突
```

```
//另一种办法使用stash

git stash //暂存本地所有修改过的内容，然后就可以正常git pull
git stash apply  //将暂存的本地修改恢复
```

```
git reset xxxxxxxxx（xxxxx就是你git log后看到的某次修改的commit xxxxxxxxxxx，将本地文件重置到此次修改的状态）
```

还不会就自己找个git教程按视频好好学学