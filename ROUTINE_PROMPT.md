# Routine 提示詞：每日台股導向市場簡報

你是在一個 Claude Code Routine 中自動執行，沒有人會在過程中審核你的每一步。
請完整、依序執行以下所有步驟，最後把成果寫進 repo 並推送。時區一律以 UTC+8（台北時間）為準。

## 步驟 0：確認日期
取得今日 UTC+8 日期，格式為 YYYY-MM-DD（以下稱 {{TODAY}}）。
今日簡報的檔名為 archive/{{TODAY}}.html。

## 步驟 1：蒐集新聞（用 web search）
搜尋並彙整「今日（UTC+8）」實際發生或揭露的消息，排除舊聞。各區範圍與權重如下：

- 台灣（核心）：5–7 則。央行與政策、財經、半導體與電子供應鏈、權值股動態、外資進出。
- 美國（次要，可複委託操作）：3–5 則。聯準會與利率、總經數據（CPI/就業/GDP）、科技與權值股、與台股電子股連動之消息。
- 日本（參考）：2–3 則。日銀政策、日圓匯率、對台股供應鏈影響。
- 香港（參考）：2–3 則。港股與中國經濟連動、金融監管。

另外查詢：
- 台股三大法人（外資、投信、自營商）最近一個交易日買賣超金額（億元）與方向，以及外資期貨未平倉。
- 權值股觀察清單當日表現與相關消息：台積電(2330)、聯發科(2454)、鴻海(2317)、廣達(2382)，以及被動元件族群。
  （此清單為固定觀察名單，每日都要逐一檢視，即使無消息也要列出該標的。）

## 步驟 2：撰寫規則
- 影響欄一律使用「利多／利空／中性」三選一。
- 時間欄格式統一為 HH:MM（24 小時制，UTC+8）；僅知日期不知時刻填「全日」。
- 每則摘要以一句話為原則，事實在前，預期/觀點以「（市場預期：…）」標註於後。
- 各表格列數須落在指定範圍；當日某區無重大消息時，仍保留該區並填一列「（今日無重大消息）」。
- 涉及個股或方向研判時，為資訊整理性質，不作投資建議。
- 來源欄請附上可點擊連結（<a> 標籤）。

## 步驟 3：產生今日 HTML
讀取 archive/_template.html，將下列佔位符替換為實際內容，產生 archive/{{TODAY}}.html：

- {{DATE}} → 今日日期 YYYY/MM/DD
- {{GENERATED_AT}} → 產出時間 YYYY/MM/DD HH:MM
- <!-- TAIWAN_NEWS_ROWS --> → 台股焦點表格列，每列格式：
  <tr><td class="time">時間</td><td>事件</td><td class="src"><a class="src-link" href="連結">來源</a></td><td>摘要</td><td class="tag up|down|neutral">利多|利空|中性</td><td>族群/標的</td></tr>
  （影響為利多用 class="tag up"、利空用 "tag down"、中性用 "tag neutral"）
- <!-- INSTITUTION_ROWS --> → 三大法人列，每列：
  <tr><td>外資/投信/自營商</td><td class="num up|down">±金額</td><td class="tag up|down">買超|賣超</td><td>備註</td></tr>
  （買超/正值用 up，賣超/負值用 down）
- <!-- INSTITUTION_NOTES --> → 外資期貨未平倉與重點，每點一個 <li>…</li>
- <!-- WATCHLIST_ROWS --> → 權值股列，每列：
  <tr><td>台積電(2330)</td><td class="num up|down">漲跌</td><td>消息</td><td class="tag up|down|neutral">影響</td></tr>
- <!-- US_NEWS_ROWS --> → 美股焦點列，格式同台股焦點，最後一欄為「對台股連動」
- <!-- JAPAN_NEWS_ROWS --> 與 <!-- HK_NEWS_ROWS --> → 四欄列：
  <tr><td class="time">時間</td><td>事件</td><td class="src"><a class="src-link" href="連結">來源</a></td><td>摘要</td></tr>
- <!-- CROSS_MARKET_NOTES --> → 跨市場連動分析，至少三點 <li>：美股→台股、匯率、其他
- <!-- MOOD --> → 利多 / 利空 / 中性 三選一
- <!-- OVERVIEW_NOTES --> → 今日總覽各點 <li>：台股關注重點 2–3 點、美股關注重點 1–2 點、風險提示 1–2 點

## 步驟 4：更新索引 manifest.json
讀取 repo 根目錄的 manifest.json，在 entries 陣列「最前面」新增一筆：
  { "date": "{{TODAY}}", "mood": "整體氛圍（利多/利空/中性）+ 一句話重點" }
若該日期已存在則覆蓋該筆，不要重複新增。保留所有歷史 entries，不可刪除。

## 步驟 5：完成的定義（commit 並 push）
- 確認 archive/{{TODAY}}.html 已建立、內容完整、所有佔位符都已替換（不可殘留任何 <!-- XXX --> 註解佔位符）。
- 確認 manifest.json 已更新且為合法 JSON。
- 將這兩個檔案 commit，commit 訊息為「每日市場簡報 {{TODAY}}」，並 push。
- push 完成即視為任務完成。

立即執行以上所有步驟。
