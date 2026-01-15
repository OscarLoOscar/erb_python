git branch
-> master

---

-> main (Last branch)
->Step 1 (變左 Main)
幾個人 start at Step1

每個人 create 屬於自己既 Branch ：

````

---

1. 專業命名公式 (Prefix Convention)最標準嘅做法係用 「類別 / 功能名稱」 或者 「類別 / 開發者名 / 功能」。A. 簡單直覺版：按功能分類如果你哋 Team 唔大，直接用功能分類就得：feature/ : 新功能 (例如 feature/login-page)fix/ 或 bugfix/ : 修復 Bug (例如 fix/header-overlap)hotfix/ : 緊急修復（通常指直接喺 Production 發現嘅大鑊嘢）refactor/ : 重構代碼（冇加新功能，只係寫得靚啲）docs/ : 只係改文件B. 多人協作版：加上開發者個名（推薦！）為咗一眼睇出個 Branch 邊個開，可以喺前面加多一層：格式： 類別/名-功能例如： * feature/ken-add-cartfix/mary-navbar-colorrefactor/tom-api-logic

---

2. 點解要咁樣命名？ (30歲版職場思維)想像 Git Branch 命名就係**「公司嘅入數單據」**：可追溯性：三個月後見到個 feature/payment-gateway，你即刻知呢度係整緊畀錢功能。避免衝突：如果兩個人都想改 Navbar，一個叫 mary/nav，一個叫 ken/nav，就唔會因為 Branch 撞名而推唔到 Code。自動化友善：好多專業工具（例如 Jenkins, GitHub Actions）可以根據你的 Prefix（如 feature/*）自動行測試，幫你慳返好多手動功夫。

---

3. 實戰建議清單 (Markdown Table)情況建議命名解釋你要整新嘢feature/yourname-task清楚知道邊個喺度開發緊咩功能。你要改錯字 / 小 Bugbugfix/issue-id如果有 Jira 或 Trello，可以用返個 Task ID（如 bugfix/102-typo）。你想試新嘢（可能唔要）experiment/try-new-api話畀人聽呢個 Branch 係實驗性質，唔好隨便 Merge。你要更新 Readmedocs/update-guide唔關 Code 事，純粹更新文件。

---

💡 額外小貼士 (Best Practices)
- 全細寫：Git 對大細寫有時會混亂，建議統一用細寫。
- 用橫線 - 而唔係底線 _：喺 URL 網址或者終端機睇起嚟更清楚。
- 唔好太長：雖然要清楚，但唔好寫成 ```feature/ken-fix-the-navbar-color-on-iphone-15-pro-max```，簡短扼要就得。
````

---

1. `git branch`
2. `git switch XXX`(by person)
3. Someone `git switch ZZZ` -> `git commit` (Someone 認領左 branch ZZZ)
4. 最後有一個人`git merge`

---

YYY local :
main --> YYY1 --> YYY2

XXX local :
main --> XXX1 --> XXX2 --> XXX3

ZZZ local :
main --> ZZZ1

PM(XXX) `git pull YYY` ,`git pull ZZZ`
XXX local:
main --> XXX1 --> XXX2 --> XXX3 --> `git merge origin`
main --> origin/YYY1 --> origin/YYY2 --> `git merge origin`
main --> origin/ZZZ1 --> `git merge origin`

YYY2 and ZZZ1 merge to XXX3, 再`git push -u origin main`
XXX3 , YYY2, ZZZ1 --> group to main branch

---

---

Git 多人協作分支管理指南

#### 1. 專案初始狀態所有開發者由 main 分支（原 master）出發，並基於 Step 1 的狀態開始各自的工作。

#### 2. 專業命名規範 (Naming Convention)為咗方便管理，我哋採用 「類別/名-功能」 的命名格式：類別命名範例適用場景 Featurefeature/xxx-login 開發新功能 Bugfixbugfix/yyy-header 修復已知 BugRefactorrefactor/zzz-api 重構代碼（不影響功能）Docsdocs/readme-update 僅更新文件

#### 3. 開發者 Git Flow 協作流程 (Mermaid 視覺化)以下展示 XXX (PM/整合者)、YYY 與 ZZZ 三人的開發路徑，以及最後如何匯總至 main：程式碼片段

```mermaid
gitGraph
    commit id: "Step 1 (Main)"

    branch feature/YYY
    branch feature/XXX
    branch feature/ZZZ

    checkout feature/YYY
    commit id: "YYY1"
    commit id: "YYY2"

    checkout feature/XXX
    commit id: "XXX1"
    commit id: "XXX2"
    commit id: "XXX3"

    checkout feature/ZZZ
    commit id: "ZZZ1"

    checkout main
    merge feature/YYY id: "Merge YYY's work"
    merge feature/ZZZ id: "Merge ZZZ's work"
    merge feature/XXX id: "Merge XXX's work (Final Push)"
```

#### 4. 實戰指令步驟 (Step-by-Step)

##### 第一步：同步與切換分支每位開發者先確保本地有最新代碼，並建立自己的分支：

```Bash
git checkout main
git pull origin main
git switch -c feature/yourname-task
```

##### 第二步：開發與提交 (Local)在各自的分支上進行開發：YYY: commit YYY1 -> commit YYY2XXX: commit XXX1 -> commit XXX2 -> commit XXX3ZZZ: commit ZZZ1 第三步：整合與合併 (PM/Leader 角色)當所有人完成後，由 XXX (PM) 負責將各分支內容拉取 (Pull) 並合併 (Merge) 到本地 main：切換回 Main: git checkout main 合併 YYY: git pull origin feature/YYY (將遠端 YYY 的進度拉入本地 main)合併 ZZZ: git pull origin feature/ZZZ 解決衝突 (Conflicts): 如果 XXX3 與其他提交有撞位，需手動修正。推送最終結果:Bashgit push -u origin main

💡 核心重點小貼士全細寫、用橫線 -：例如 feature/ken-add-cart，唔好用大細寫混合或底線。先 Pull 後 Push：喺 Merge 之前，一定要先 git pull 攞咗最新嘅 code 翻嚟先，減少 Conflict 大爆發嘅機會。原子提交 (Atomic Commit)：一個 Commit 只做一件事，唔好將成個星期嘅 code 一次過塞入一個 commit。

---

### Git:

支箭綠色：working directory ,Staging Area ,Repository(local Repository) 係一樣，同一個 directory
Working Directory(local):(untracked file) manage.py -> `git add.` ,then pass to `Staging Area`

Staging Area: can `git reset` to local

Repository(local Repository) : `git commit` from Staging Area to Repository (but this Repository is local , not git Repository)

Repository(Remote Repository):`git push` form Repository(local) to Repository(Remote Repository)
`git pull` form Repository(Remote Repository) to Repository(local)

---

### Basics

#### You can create a repository with either of the following commands.

| 指令   | 用法                                                                                                        | 用途                                                                                                                     |
| ------ | ----------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| clone  | git clone https://github.com/nesi/perf-training.git <br/>or <br/>git clone git@github.com:aaronwai/erb4.git | Copies a remote repository into your current directory.                                                                  |
| init   | git init                                                                                                    | Creates a new empty repo in your current directory.                                                                      |
|        |
| add    | git add <file1> <file2>                                                                                     | Adds <file1> and <file2> to the staging area.                                                                            |
| -      | git add \*.py                                                                                               | Adds all python files in the current directory to the staging area.                                                      |
| status | git status Lists changes in working directory, and staged files.                                            |
| commit | git commit                                                                                                  | Records everything in the staging area to your repository. The default text editor will prompt you for a commit message. |
| -      | git commit -m "Commit message"                                                                              | Records everything in the staging area to your repository with the commit message "Commit message"                       |
| -      | git commit --amend                                                                                          | Modify last commit instead of creating a new one. Useful for fixing small mistakes.                                      |
| log    | git log                                                                                                     | Prints commit history of repo.                                                                                           |
| -      | git log <filename>                                                                                          | Prints commit history of <filename>.                                                                                     |
| reset  | git reset                                                                                                   | Removes all files from staging area. (Opposite of git add)                                                               |
| -      | git reset <filename>                                                                                        | Removes <filename> from staging area.                                                                                    |

---

### Remote

#### By default, fetch, pull and push will operate on the origin repo. This will be the repo you cloned from, or set manually using git branch --set-upstream-to <origin>.

| 指令  | 用法                      | 用途                                                                                                        |
| ----- | ------------------------- | ----------------------------------------------------------------------------------------------------------- |
| fetch | git fetch                 | Gets status of origin. git fetch does not change your working directory or local repository (see git pull). |
| -     | git fetch <repo> <branch> | Get status of <repo> <branch>.                                                                              |
| pull  | git pull                  | Incorporates changes from 'origin' into local repo.                                                         |
| -     | git pull <repo> <branch>  | Incorporates changes from <repo> <branch> into local repo.                                                  |
| push  | git push                  | Incorporates changes from local repo into origin.                                                           |
| -     | git push <repo> <branch>  | Incorporates changes from local repo into <repo> <branch>                                                   |

---

### Branches

#### At an introductory level, it is best to avoid workflows that lead to multiple branches, or requires merging.

| 指令     | 用法                       | 用途                                     |
| -------- | -------------------------- | ---------------------------------------- |
| branch   | git branch                 | List branches.                           |
| -        | git branch <branch-name>   | Create new branch <branch-name           |
| checkout | git checkout <branch-name> | Switch to editing branch <branch-name>   |
| merge    | git merge <branch-name>    | Merge <branch-name> into current branch. |

---

### erb4:

`git status` -> 睇 local 係咩 branch

`main ? 'upper arrow' 5 'downer arrow' 7`
`upper arrow` :
`downer arrow` :

Example : 2 file untracked
?->`Untracked file` , need `git add` to track the file
how to backup ones of the file?
`git add xxx.yyy(file name)`

`git status` again
only one file untracked

`changes to be commit:`
`use git restore --staged <file> ..." to unstage`

## 小心 `git restore`：

`git restore`: `git restore --staged <file name>`
`git restore` :

3 個可能性：

1. Staging Area -> Working Directory
2. Local Repo -> Staging Area
3. Local Repo -> Working Directory

## 有機會 overwrite file，用 empty file overwrite

`rm <file>` -> delete file
再`ls -l` , `'file` was delete

1. 一係 delete staging area's file
2. 係 staging area `git restore 'file name'`

Fail to delete : `git rm 'file name'`:

---

`error : the following file has changes staged in te index:
'file name'
(use --cached to keep the file, or -f to force removal)`

Success delete file : `git rm -f 'file name'`

Create file :
`git ls-files erb9.txt`
or
`echo "write some Context" > 'file name'` ,'>' 加 D 野

`echo "write some Context" >> 'file name'`,'>>' append new line

---

Check how many files in Staging Area :
`git ls-files`
check specific file :
`git ls-files <<file name>>`

Delete Staging Area file , 唔會 delete local Working Directory:
`git rm --cached -rf erb8.txt`

---

`git ls-files erb9.txt`
改 file name:
`mv erb9.txt erb9`

---

1. `mv erb9.txt erb9`
2. `git ls-files erb9.txt` , result : erb.txt
3. `git add erb9.txt` , 加個空 file 落 staging area
4. `git ls-files erb9.txt` , result : show nothing
5. `git add erb9`
6. `git ls-files erb9`

---

`git status -s` : s : short hand , result :
A erb9
?? erb8.txt

第一行 ： staging area
第二行：working directory

加入 staging area：
`git add erb8.txt`
再`git status -s` , result :
A erb8.txt
A erb9

人手 modify file,
`git status -s` ,result:
A erb8.txt
AM erb9 , A -> Add , M -> Modify,master 出圓圈 ⭕️
`git add erb9`
`git status -s` , result:
A erb8.txt
A erb9

---

`git diff` -> 睇邊個 file 邊行改左

`git log` -> show commit 過咩

`git log --oneline` -> 睇到 message 同 git push number

`git log --oneline --reverse` 同`git log --oneline`，但排序由新到舊

`git log --oneline --stat` 睇到每個 commit 改左乜

`git log --oneline --patch` ,show all code
但太長，所以
`git log --oneline -3` , 睇最近個 3 個 commit

Filter：
`git log --oneline --author="'author name'"` , short by author name

`git log --oneline --author="email"` , short by email

`git log --oneline --after="three months ago"` (3 個月前到今日)

`git log --oneline a45e..6611` ,用 number 搵 git push number

`git show HEAD-2`,睇下個頭指住乜 number result :123456 (HEAD->)...

`git show 25f9` / `git show 6611` ,

`git log --online --all` , 睇晒 all branch

`git checkout master` , 倒帶去番 master branch

---

`git merge origin/master` , 但要 commit 左先可以 merge ,想 force `git merge`要行`git stash` : add 完唔想 commit，可以 run`git stash`

`git stash apply` , `git pull`完要打，令到 working directory 可以令 file 一置

`git stash apply`完，打`git stash pop` , delete `git stash` record
