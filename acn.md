list 5 special point of django
how to set up git
how to add username and email
how to build and switch branch in git : git switch
pros of python ide

---

list 5 special point of django:

- Batteries Included:唔洗自己搞用戶認証，ORM，backend唔洗搵咁多第三方套件
  - 用戶認證系統 (Auth System)
  - RSS 訂閱生成(website content 變成Feed（訂閱源），等reader可以用第一時間變到article update)
  - Sitemaps（Google爬蟲更全面收到你website既content，for SEO）

- Admin Interface
- ORM Object Relational Mapping
- High Security（防範多種comment網路攻擊） - XSS , CSRF,SQL Injection
- MVT (Model ,View , Template)

---

How to setup git:

ssh-keygen -t ed25519 -C "your_email@example.com"

github account
settings
SSH and GPG keys
press New SSH key
copy and paste .pub content

git config

1.  git config --global user.name =="your name"==
2.  git config --global user.email ==your_email@example.com==
    --- Optional
3.  git config --global init.defaultBranch main
4.  git config --global pager.branch false
5.  git config --global pager.diff false

---

python IDE:
VScode:

- Extension Marketplace(可以裝好多extension方便你做野，example： Ayu, Auto Close Tag，Code Spell Checker,Django,Docker)
- 優秀效能同開發體驗:
  - 開app速度快，打`code .`就秒著，或者將folder變左workspace都著得好快 ,
  - 內置terminal，唔洗不停switch 唔同window
- Git深度整合，唔洗記晒所有git command：側邊欄有UI，令你git stage , git commit , git pull/push。裝gitLens可以check番每一行code係邊個幾時寫
- 有debugger
- editor
