# Git

## 目錄
* [Setup](#Setup)
* [Repository & Local](#Repository-amp-Local)
  * [tracking file](#tracking-file)
  * [file name](#file-name)
  * [commit](#commit)
  * [發佈到雲端版控](#發佈到雲端版控)
* [Stash](#Stash)
* [Branch](#Branch)
  * [basic](#basic)
* [Git config](#Git-config)
  * [專案設定](#專案設定)
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

### file name

檔案中`test1.txt`假設改`Test1.txt `

因為檔名如果只是大小寫不同，這就會讓Git 無法分辨

![](https://i.imgur.com/JlEOYQV.png)

![](https://i.imgur.com/7nnATQb.png)

![](https://i.imgur.com/nShXJgJ.png)

**解決方式一:**

```
git mv test1.txt TEST1.txt
```

或是乾脆一開始不直接更改檔名，直接使用`git mv`也可以同時修改檔案名和Git
```
git mv oldFileName newFileName
```

![](https://i.imgur.com/Kfi5I1I.png)



**解決方式二:**

修改全域變數
```
git config --global core.ignorecase false
```
只關閉單一專案設定
```
git config --local core.ignorecase false
```


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


### 發佈到雲端版控

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

## Branch

當多人協作時，分支的建立可以分離避免主要版本受到過多影響，而每個分支可以在細分為功能分支，在該分支確定合併成功後，再跟主分支`master`合併。

### basic
查看當前的分支有多少，分支旁邊有`*`代表當前所在分支

```
git branch
```
![](https://i.imgur.com/GQ5bOQg.png)

我們建立一個develop的分支
```
git branch develop
```
![](https://i.imgur.com/aKdiNxM.png)

切換至develop分支
```
git checkout
```

現在我們正確在develop分支
![](https://i.imgur.com/174kP98.png)

![](https://i.imgur.com/UZkhKF5.png)

假設需要一下新增一個功能，我們新增一個index.js模擬
![](https://i.imgur.com/IiXcYMu.png)

可以看到在develop分支中的檔案有index.html、test1.txt、index.js

![](https://i.imgur.com/AEFpHAI.png)

切回master分支中，確實與develop不同版本
![](https://i.imgur.com/j0FcJFZ.png)

那現在假設測試版本都可以正常運作準備上線，我們需要把develop分支的版本合併(merge)至master分支中


:exclamation:  需切回到master分支才能合併分支
```
git merge develop
```

![](https://i.imgur.com/wEeGLEM.png)

![](https://i.imgur.com/NeP8WCu.png)

這樣master和develop都指向當前最新的版本"add index.js"
![](https://i.imgur.com/svfQQM8.png)


## Git config

我們可以輸入以下指令查看git 設定檔

```
vim ~/.git
```
然後按下`tab`就會顯示相關檔案
![](https://i.imgur.com/zGZAlDe.png)

而最重要的在`.gitconfig`

![](https://i.imgur.com/loNloq0.png)

也可以使用
```
git config --list
```
![](https://i.imgur.com/G1zYoMQ.png)

### 專案設定

假設我的Git 全域設定是叫做`Dennis xxx`，但是我在這個專案中想要使用別的名稱


我們就可以使用：
```
cd <project>
git config --local user.name "username"
```
![](https://i.imgur.com/C5Nx8Gs.png)


查看專案中的`.git`
```
cd <project>
vim .git/config
```

![](https://i.imgur.com/Pg1mcde.png)


假設今天想刪掉區域變數了，想改回使用全域的名字"Dennis XXX"
```
git config --local --unset user.name
```
![](https://i.imgur.com/dsCIbSF.png)

![](https://i.imgur.com/nVevpsY.png)

刪除成功～～～

---