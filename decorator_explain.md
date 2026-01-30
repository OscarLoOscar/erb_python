```mermaid
graph TD
    Start((開始執行)) --> Step1[<b>Step 1:</b> 呼叫 param 10]
    Step1 --> Step2[<b>Step 2:</b> param 建立一個空間<br>儲存 count=10 <br/>並回傳 counter 函數]
    Step2 --> Step3[<b>Step 3:</b> 呼叫 counter add]
    Step3 --> Step4[<b>Step 4:</b> counter 建立 inner <br/>小助手 <br/>inner 記住咗 fn=add <br/>同埋 count=10]
    Step4 --> Step5[<b>Step 5:</b> add 標籤正式指向<br/> inner]
    Step5 --> Step6[<b>Step 6:</b> 執行 add 1, 2 <br/>即係執行 inner 1, 2]
    Step6 --> Step7[<b>Step 7:</b> count 10 + 1 <br/>變成 11]
    Step7 --> Step8[<b>Step 8:</b> Print 訊息到畫面]
    Step8 --> Step9[<b>Step 9:</b> 執行原本嘅 add 1, 2 得到 3]
    Step9 --> LastStep((<b>Last Step:</b> print 得到嘅 3))
```

---

**Step 1: Pass 咗啲乜？**
當你行 `add = param(10)(add)` 嘅時候，其實係分咗兩步：

`param(10)`: 你 pass 咗整數 `10` 入去。呢一吓會產生一個「環境」，入面嘅 `count` 係 `10`。佢回傳咗 `counter` 呢個函數出嚟。

`(add)`: 隨即你將 `add` function pass 入去頭先回傳嗰個 `counter` 度。呢一吓會產生 `inner，而呢個` `inner` 係睇到 `count=10` 嘅。

**Last Step: Output 咗啲乜？**
最後 `print(add(1, 2))` 會喺畫面出現兩行嘢：

第一行（由 `inner` 入面嘅 `print` 產生）： F`unction add called 11 times` (因為 `nonlocal count` 由 10 加咗 1，所以係 11)

第二行（由最外面嘅 `print` 產生）： `3` (因為 `fn(1, 2)` 回傳咗 3，最後被最外面層 print 出嚟)

點解要咁寫？（三層結構）
第一層 `param`: 係「設定層」。用嚟決定計數器由幾多開始計（例如你想由 0 開始定由 10 開始）。

第二層 `counter`: 係「裝修層」。負責接收你要裝飾嘅 function。

第三層 `inner`: 係「執行層」。每次你叫 `add()` 佢都會行，負責計數同叫原本部機做嘢。

---

## Python Designing Pattern != Java Designing Pattern

```python
@param()
def add(a,b=0):
return a+b

@param()
def mult(a,b=1):
return a\*b

Ab()
Ac() ,可以獨立改A，b,c ,factor out function
```
