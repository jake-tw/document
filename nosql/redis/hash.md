# Hash

- Key with name fields
    - 類似用 Key 分組後的 mini key value pair
    - { key: { field1 : value1, field2 : value2 } }

- Single level
- 提供特定的指令操作 Fields
- 可動態新增或移除 Fields，為了有效率的操作大量資料，基本上沒有結構
- 所有資料都只能用 String 來儲存，沒有嵌套概念，Fields 不能設定 list, set, sorted set, hash
- 刪除 Key，相關 Fields 都會被跟著刪除
- Use cases
    - Rate limiting
        - 如下建立每日 Api 訪問上限

            ```cmd
            HMSET date:20200101 "/booking/{id}" 100
            ```

        - 每次訪問檢查剩餘次數並遞減 1

            ```cmd
            HINCRBY date:20200101 "/booking/{id}" -1
            ```

        - 設定過期時間，避免佔用記憶體

            ```cmd
            EXPIRE date:20200101 86400
            ```

    - Session cache
        - 儲存跨服務的 Session

        ```cmd
        HMSET session:3a6r ts 1518132000 host 127.0.0.1
        ```

        - 記錄訪問次數

        ```cmd
        HINCRBY session:3a6r requests 1
        ```

        - 設定 Session 到期時間

        ```cmd
        EXPIRE session:3a6r 1200
        ```