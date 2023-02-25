# Git

## 目錄
* [Setup](#Setup)
* [Repository & Local](#Repository-amp-Local)
  * [tracking file](#tracking-file)
  * [commit](#commit)
  * [push to server](#push-to-server)
* [Stash](#Stash)

---

## Setup
[Download Git](https://git-scm.com/)
![](https://i.imgur.com/xw84LZ5.jpg)

有好的GUI搭配使用Git可以增加工作效率
[Download GUI](https://desktop.github.com/)
![](https://i.imgur.com/i4x2cC6.png)

---

## Repository & Local

![](https://i.imgur.com/dHo1wxZ.png)

>https://git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository

上面的工作狀態我們可以分為四個部分
1. Untracked 未加入追蹤的本地檔案
2. Unmodified 以加入追蹤但尚未修改
3. Modified 修改後但尚未commit
4. Staged 已完成commit的動作，這樣就完成一次版本的映射

而每當完成commit後當改動檔案時，Git會再次需要添加索引至檔案中也就是`git add <fileName>`的動作

### tracking file


![](https://i.imgur.com/yAKGFSw.png)

我們建立一個檔案`test1`
```bash
touch test1
```

![](https://i.imgur.com/zBmvbfA.png)

輸入以下指令查看是否有未加入追蹤的檔案
```bash
git status
```

![](https://i.imgur.com/siIy53A.png)


我們可以透過`git add <fileName>`追蹤該檔案
```
git add <fileName>
```


![](https://i.imgur.com/2OCHR0B.png)

可以看到`test1.txt`已被加入追蹤
![](https://i.imgur.com/jxvWptY.png)

假設今天很多個檔案，使用`git add <fileName>`，可能效率不是太好

輸入`git add .`可以幫助我們追蹤所有未追蹤的檔案
```
git add .
```
![](https://i.imgur.com/myQCmRy.png)

### commit

接著我們需要推送commit到暫存區，輸入`git commit -m "text"`
```
git commit -m "text"
```

![](https://i.imgur.com/yXyptkf.png)


輸入`git log`查看目前的commit紀錄
```
git log
```

![](https://i.imgur.com/CZfdqtX.png)

再commit 一次

![](https://i.imgur.com/HdJY0YD.png)

這時候我想把log圖像化的話可以使用

```
git log --graph
```

![](https://i.imgur.com/5v6Bw2B.png)

當前最新的commit為`add introduction`，也就是最新一筆，可以使用:
```
git log -1 --graph
```

![](https://i.imgur.com/CZgZCfX.png)


### push to server

這邊我們必須先說雲端的版本存放不是只有Github，像是AWS、Gitlab等等都可以

先建立一個主分支
```
git branch -M master
```
接著我們需要設定連線遠端的source也就是我們目標的來源
```
git remote add <目標暱稱> <url>
git remote add origin <url>
```

接著把我們的master分支推送至目標

```
git push -u origin master
```

![](https://i.imgur.com/DF05ibZ.png)


---

## Stash
這是最新版本的index.html
![](https://i.imgur.com/T4SOZLo.png)

今天假設你在寫個功能，但是臨時需要把上一版小地方修改一下趕快重新上線

這是更改過後的index.html
![](https://i.imgur.com/C3VIc9l.png)

我們可以選擇把剛剛寫到一半的功能刪掉

```
git restore <fileName>
```

![](https://i.imgur.com/cZYNztQ.png)

成功回復至最新版本
![](https://i.imgur.com/Xhtw6IA.png)

---

也可以使用下列指令暫存我們的進度

```
git stash
```
![](https://i.imgur.com/Ogs85st.png)

修改bug後趕快上線的版本
![](https://i.imgur.com/BlDKKeN.png)
趕快上commit
![](https://i.imgur.com/euFSvjL.png)

接著我們可以專心把剛剛進度從暫存區中拿回來，先看一下暫存列表

```
git stash list
```

![](https://i.imgur.com/xaDca1s.png)

使用下列指令把剛剛進度還原，因為我們只要恢復最新的stash，所以使用pop可以把最新的stash取出
```
git stash pop
```


因為當前的版本為緊急修改bug的版本，而暫存的版本因為沒有"bug fix version"的程式碼又與當前版本衝突(conflct)

![](https://i.imgur.com/6FNOLyO.png)


輸入以下指令比對版本差異
```
git diff index.html
```

![](https://i.imgur.com/9ulXyVR.png)

可以看到Git幫我們比對差異的地方，也很清楚哪一段程式碼是當前版本衝突的地方
![](https://i.imgur.com/xFP4lFf.png)

簡單的衝突可以使用vim編輯器修改

`>>>`、`<<<`、`===`都是在告訴我們這一段是什麼狀態的程式碼，一定要刪掉這些提示的訊息，然後再看我們要保留哪些程式再進行儲存

![](https://i.imgur.com/Oe7NzUM.png)

當然也可以使用VS Code編輯

![](https://i.imgur.com/xSLeJxJ.png)


儲存完後趕緊加入追蹤
![](https://i.imgur.com/qrNVqMl.png)

commit "merge stash"
![](https://i.imgur.com/g96tuJa.png)

可以看到成功的推送commit
![](https://i.imgur.com/sepeht9.png)

我們再回去看index.html就是我們合併成功的版本了
![](https://i.imgur.com/aEd1w6c.png)

---