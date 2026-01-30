<h2>Python Virtual Environment :</h2>

---

1. all folder / file under project(one folder)
   - package.json
   - node_module

2. project folder
   - .env
   - Bean

10 projects but same python version
will crush : one Numpy , one DJango

---

wailung version:

- Project Folder
- ~/.pyenv
  (Prthon 同Project脫勾)
- Virtual Env -> env 都脫勾
  -> 一個Virtual Env可以apply落唔同project
  唔同每個project都build一個.env folder
  -> 可以重用
  **example : .virtualenvs/erb7/lib/python3.10/site-packages/Django**

---

```bash
mkvirtualenv erb8
```
