```
Author: Jyun Teng Huang
Date: 2026/03/12
```

# Git/Github Tutorial

## 1. Git Extension

- Git
  可以透過下方連結下載使用
  [git download link](https://git-scm.com/install/windows)

- Git-LFS
  當我們`git commit`大型檔案像是圖片、log等等的，git-lfs 會把這個大檔案攔截下來，改放到一個獨立的 LFS 快取區。
  當我們使用`git push`推送到remote-repository的時候，LFS 快取區會被推到GitHub專門存放大型檔案的伺服器。

- Git-filter-repo
  當我們想要清除過往某些特定的檔案，會用到這個tool。

```
pip install git-filter-repo # manage git history
git lfs install             # manage large file (>100MB)
```

## 2. Git Config

替換名稱與電子郵件成你的github帳密。雙引號要保留。

```
# global domain config, all the repo will use this setting
git config --global  user.name "使用者名稱"
git config --global  user.email "使用者電子郵件"
```

```
# local domain config, only current repo will use this setting
git config --local user.name "使用者名稱"
git config --local user.email "使用者電子郵件"
```

檢查是否設定成功。

```
git config -l
```

## 3. Github Connect Config

檢查當下資料夾連線到的Github庫設定。如果我們是使用`git clone`去Github上抓取repo的話，git remote會被自動設定。

```
git remote -v   # Check the remote connection
```

輸入後的結果如下。可以看到origin就是代表你可以透過後面的網址去抓或者上傳資料。

```
origin  https://github.com/nckuLCL/LCL-PPG-EEG-System.git (fetch)
origin  https://github.com/nckuLCL/LCL-PPG-EEG-System.git (push)
```

更多操作可以參考:

1. https://git-scm.com/book/zh-tw/v2/Git-%E5%9F%BA%E7%A4%8E-%E8%88%87%E9%81%A0%E7%AB%AF%E5%8D%94%E5%90%8C%E5%B7%A5%E4%BD%9C

## 4. 基礎操作

開發流程是先從main拉出一個自己的`dev`分支，改完後再merge到主分支。

### 抓Github Repo.

去Github複製repo url再用下面的指令就可以抓到本地。

```
git clone <repo-url>
```

### Commit

透過`git add`把所有檔案更新，上傳本地git庫。

```
git add .
git commit -m "輸入更改了甚麼"
```

### Branch

列出有哪些分支。

```
git branch
```

branch 改名子，不用保留括弧。

```
git branch -m <old-name> <new-name>
```

複製當前branch新增一條分支。

```
git branch <new-branch>
```

跳轉branch

```
git checkout <your-branch>
```

刪除branch

```
git branch -d <your-branch>
```

更多操作可以參考:

1. https://git-scm.com/docs/git-branch/zh_HANS-CN
2. https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging

### merge

將`<merged-branch>`融合進當前branch。

```
git merge <merged-branch>
```

如果兩個分支都有改動同個檔案會產生Conflicts，需手動排除。
排除完檔案後重新`commit`就會merge兩個branch了。記得刪除無用的branch。

```
git branch -d <your-branch>
```

更多操作可以參考:

1. https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging

### push and pull

`pull`是將雲端庫的資料抓進本地。`push`是將本地的git推送到雲端。

```
git pull
git push
```

推送特定的branch

```
git push origin <本地分支名稱>
# example
git push origin dev
```

清除本地進度，直接跟雲端庫同步。

```
git reset --hard HEAD
git push
```

## 5. 檔案追蹤清除

### Git LFS

專門處理大型檔案，像是照片，權重等。
安裝 `lfs`。

```
git lfs install
```

追蹤檔案設定範例。設定完成可至`.gitattributes`查看，之後透過`add`與`commit`更新至git庫。

```
# 追蹤特定副檔名
git lfs track "*.psd"
git lfs track "*.mp4"

# 或是追蹤某個特定的大檔案
git lfs track "assets/huge_video.mov"

# 追蹤特定資料夾及其所有子目錄
git lfs track "large_assets/**"

# 取消追蹤特定資料夾及其所有子目錄
git lfs untrack "large_assets/**"
```

### Git-filter-repo

當我們要把檔案從 LFS 轉回一般 Git（或者反過來）時，過去那些用舊規則產生的歷史紀錄通常已經沒有留著的意義了，甚至如果繼續留著，GitHub 依然會因為歷史紀錄裡有舊檔案而報錯。這時候，會搭配 git-filter-repo來清除那些檔案追蹤紀錄。

掃描歷史紀錄中所有路徑包含 waveform 的東西，然後把牠們「反向刪除（Invert）」，也就是從歷史中徹底刪除。

```
git filter-repo --path-match <path-to-files-or-folder> --invert-paths --force
# example
git filter-repo --path-match waveform --invert-paths --force
```

刪除完成後要重新追蹤remote repo，並且重新`commit`一個新的版本。

```
git remote add origin <https://github.com/url/xxxxx.git>
git add .
git commit -m "refact: del xxxxx"
git push
```

## 6. HW

`*`代表commit，每個人從`main branch`拉出自己的分支做對應的操作。最後合到`main branch`。

```
* (HEAD -> main) Merge branch
|\ \ \
| | | * feat: Subtractor    (branch user 3)
| | * | feat: Multiplier    (branch user 2)
| * | | feat: Adder         (branch user 1)
* | | | feat: hello world   (branch main)
|/ / /
* feat: init
```
