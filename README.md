# AI 新聞歷史存檔 — 使用說明

## 檔案
- `ai-news-archive.json`：全部歷史新聞資料（UTF-8 編碼）

## 資料格式
```json
{
  "generated": "2026-10-20",
  "description": "...",
  "editions": [
    {
      "date": "2026-10-19",        // 日期
      "edition": "evening",         // midday=中午版 / evening=傍晚版
      "label": "2026-10-19 傍晚版",
      "count": 20,
      "items": [
        {
          "category": "產業動態",   // 分類：產業動態 / 產品發表 / 研究突破 / AI 類股
          "title": "...",           // 標題（繁體中文）
          "summary": "...",        // 一句話摘要（僅 10/14 傍晚版有）
          "why": "...",            // 為什麼重要（僅 10/14 傍晚版有）
          "url": "...",            // 原文連結（僅 10/14 傍晚版有）
          "source": "...",         // 來源媒體名稱
          "pub_date": "10/19"      // 報導日期
        }
      ]
    }
  ]
}
```

## 注意事項
- 目前 8 個版本、160 則，其中 75 則有原文連結（10/13 中午版、10/13 傍晚版、10/14 中午版、10/14 傍晚版）；10/16、10/18、10/19 的版本當時只存了標題與來源，沒有留連結。
- 從 2026-10-20 開始，每一版更新都會自動把完整 JSON（含原文連結）附加到這個檔案，存檔只會越來越齊。

## 外部網頁取用範例
把 `ai-news-archive.json` 放到你的網站目錄下，前端直接 fetch：

```html
<script>
fetch('ai-news-archive.json')
  .then(r => r.json())
  .then(data => {
    // 最新一版
    const latest = data.editions[data.editions.length - 1];
    console.log(latest.label, latest.count + ' 則');
    // 全部攤平成一維陣列
    const all = data.editions.flatMap(e =>
      e.items.map(it => ({...it, date: e.date, edition: e.edition}))
    );
    // 例：只取「研究突破」
    const research = all.filter(it => it.category === '研究突破');
    // 渲染到頁面...
  });
</script>
```
