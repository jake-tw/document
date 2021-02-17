# Set

- 不重複的 String 集合
- 所有資料都只能用 String 來儲存，沒有嵌套概念，不能設定 list, set, sorted set, hash
- 忽略順序
- 支援集合操作
    - A difference B
        - 取得 A 與 B 不重複的部分，並非差集

    - A intersection B
        - 取得兩個 Set 重疊的部分

    - A union B
        - 取得兩個 Set 的所有值，並去除重複

- Use cases
    - Tag cloud
        - 加入 Tag

            ```cmd
            SADD wrench tool metal

            SADD coin currency metal
            ```

        - 查詢 Tag

            ```cmd
            SSCAN wrench 0 match *
            ```

        - 查找兩樣東西的共通點

            ```cmd
            SINTER wrench coin
            ```

        - 查詢已建立的 Tag

            ```cmd
            SUNION wrench coin
            ```

    - Unique visitor
        - 使用時間記錄每個頁面每日不重複的訪客

            ```cmd
            SADD index.html:20200101 a b c
            ```

        - 搜尋當日訪客

            ```cmd
            SSCAN index.html:20200101 0 match *
            ```

        - 設定過期時間

            ```cmd
            EXPIRE index.html:20200101 86400
            ```