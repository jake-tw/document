## Faceted search

- Attribute search
    - Disabled access availbale: yes / no
    - Medal event: yes / no
    - Event venue: String

- Traditional approaches
    - Secondary indexs
        - Maintenance
            - 新增資料時，為了保持索引的一致性，需要額外耗費資源維護索引，耗費更多的 Memory, disk, I/O etc.

        - Storage
            - 需要花費額外的空間來儲存索引

        - Multiple indexs
            - 資料庫需支援 Index intersection
            - 需要 statistics 確定如何優化查詢
            - 需保持 statistics 正確

        - Compound indexs
            - 若有多種組合搜尋條件，管理會變得很複雜

    - Full text search
        - 使用 Solr 或 Lucene 之類的全文搜尋引擎
        - Complexity
            - 增加額外部屬與管理的複雜性

        - Eventual consistency
            - 通常會有延遲，在更改資料庫的值與文本索引之間的時間差，可能會有錯誤搜尋，導致不良的使用者體驗

- Using Redis
    - Redis 不支援 Secondary indexs，但可以由以下方式來實現同樣的搜尋結果
    - Domain object
        - Disabled access
        - Venue
        - Medal event

    - Method 1 : Full scan
        - Object inspection
            - Set event data using JSON

                ```cmd
                SET event:1 "{"id":1,"name":"4x100 heats","venue":"tokyo", ...}"
                ...
                ```

            - Use scan

                ```cmd
                SCAN 0 MATCH event:*
                ```
            
            - Get event and compare in application

                ```cmd
                GET event:1
                ...
                ```

            - 在程式中將 JSON Node 逐個取出比較，只要有一項條件不符合則排除
            - 相當於關聯式資料庫中的 Full table scan，不建議在有效能要求的情況下使用

    - Method 2 : Faceted search ( Inverted index )
        - Navigation
        - Search across multiple criteria
        - Filter across multiple criteria
        - Set intersaction
            - Group by classification
                - fs 表示作用於分面搜索

                    ```cmd
                    SADD fs:disabled_access:true event:1 event:2 event:4
                    SADD fs:disabled_access:false event:3 event:5
                    SADD fs:medal_event:true event:1 event:4
                    SADD fs:medal_event:false event:2 event:3 event:5
                    ...
                    ```

            - Get intersaction

                ```cmd
                SINTER fs:disabled_access:true fs:medal_event:false
                ```

            - Get detail

                ```cmd
                GET event:1
                ```

            - 要使用分面搜索，需注意屬性的數據分佈以及搜尋條件數量
            - GET 為 O(1)，但因為結果不只一個，所以這邊是 O(N)
            - SINTER 的複雜度為 O(N*M)，搜尋的基數不同時，複雜度也會有很大的差異
            - 複雜度隨著搜尋條件增加而增加
            - 2 個條件的情況下，基數低於 50% 時會開始減少搜索成本，3 個條件則是 33%

    - Method 3 : Hashed search
        - Hashed index
            - Create a consistent Hash ( Using search attribute to create key )

                ```cmd
                SADD hfs:c3d9d001 event:1 event:2 event:4
                // disabled access true

                SADD hfs:e8275e9d event:1
                // disabled access true and medal event false
                ```

            - Search by Hash

                ```cmd
                SSCAN hfs:c3d9d001 0 match event:*
                ```

            - SSCAN 複雜度為 O(N)，但是這邊只需儲存匹配條件的組合，所以實際上還是能降低搜索成本
            - 變更屬性值的情況，需要將成員從目標 Set 中移除，重新尋找合適的 Hashed index set
            - 如果資料不是經常改變的情況，這會是一個很好的解決方案