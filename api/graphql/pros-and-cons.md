# GraphQL 優缺點

簡單介紹幾個 GraphQL 的優缺點

### 優點
1. 取得的資料由 Client 端決定，並回傳可預期的資料結構
2. 依照 Graph 連接資料，單一 Request 就完成所有查詢
3. 內省( introspection )機制，Schema 建立即完成文件，降低溝通成本
4. 無遞增版號設計，搭配 @deprecated，隨時間增加或廢棄某 field 又能避免 breaking change
5. 可利用既有的業務邏輯代碼進行開發
6. 強型別

### 缺點
1. 不小心就會陷入 N + 1 Query 問題，雖然有 DataLoader(*1) 之類的解決方案，但又會增加程式的複雜度
2. 需要另外處理(*2) Nested query 帶來的問題
3. 由於是相對新的技術，還沒有完整的 Best Practice 可以參考
4. 額外的學習成本，效能問題、錯誤處理、安全性...等，都會和原先的設計習慣有所不同，而且一不注意就會忽略 GraphQL 的優勢，設計出一套「 RESTful GraphQL」

<br>
<br>
<br>

> 1. DataLoader 在 Query 量不大時反而會降低查詢速度，建議遇到效能瓶頸時再來考慮引入。  
> 2. Maximum Query Depth, Query Complexity...etc，如果選用的框架沒有實作相關功能則需要自行處理。