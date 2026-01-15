### Installation of React

```bash
brew install node watchman yarn
```

then run

```bash
npm create vite@latest
```

create project

```bash
npm create vite@latest my-react-app -- --template react
```

**my-react-app**: your Project name

**--template react**: tell the computer that you use React

```bash
go inside the folder : cd my-react-app

install something : npm install

start server : npm run dev
```

```bash
Need to install the following packages:
create-vite@8.2.0
Ok to proceed? (y) y


> npx
> "create-vite" react_ecommerce --template react

│
◆  Use rolldown-vite (Experimental)?:
│  ● Yes (The future default Vite, which is powered by
Rolldown)
│  ○ No
```

Yes : Rolldown：係 Vite 團隊用 Rust 語言重寫嘅底層打包工具。佢嘅目標係極速，同埋統一開發（Dev）同埋打包（Build）嘅體驗。

Experimental (實驗性)：目前佢仲未係 100% 穩定。如果你係初學者，或者係想整一個用嚟做嘢嘅 Project，暫時唔好做「白老鼠」。

| 選項      | 技術背景            | 建議對象                                                            |
| --------- | ------------------- | ------------------------------------------------------------------- |
| No (推薦) | 使用傳統的 Rollup   | 初學者 / 生產環境。教學多、插件支援最齊、絕對穩定。                 |
| Yes       | 使用最新的 Rolldown | 開發狂熱者。想幫手試 Bug 或者追求極致速度，但可能會撞到插件唔兼容。 |

---

Install with npm and start now?
│ ● Yes / ○ No

揀 「Yes」 定 「No」？
揀 「Yes」 (最快)： Vite 會自動幫你行埋 npm install。你等佢行完，直接 cd my-react-app 然後 npm run dev 就可以開工。

揀 「No」 (手動)： 佢只會幫你起好啲檔案夾，你自己手動入去打指令。

推薦揀 「Yes」
既然你係新 Mac 乜都未裝，揀 Yes 可以幫你驗證埋你個 Node.js 同 npm 係咪運作正常。

Then you will see

```bash
 Scaffolding project in /Users/oscarlo/django_ecommerce/react_ecommerce...
│
◇  Installing dependencies with npm...
```

---

React 用 Tailwindcss 多，唔用 bootstrap，但 bootstrap 去 Taiowind 會易 D
`https://tailwindcss.com/`
