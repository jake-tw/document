# Big O notation and Redis commands

在 [redis.io](https://redis.io/) 的指令說明，可以查看該指令的複雜度，舉例來說，SADD 有以下敘述

```txt
Time complexity: O(1) for each element added, so O(N) to add N elements when the command is called with multiple arguments.
```

但要注意的是，即使兩個都是 O(1) 複雜度的指令，執行時間也可能不同，就算是相同的指令，也可能因為 Key 或 Value 的值不同而造成執行時間的差異。

Redis 執行命令是具有原子性( Atomic )的，要不是指令執行成功，就是回退到執行前的狀態，為了實現這一點，Redis 執行所有命令都只有單線程( Single thread )，在大多數情況下，每個命令會按照順序一一處理，所以了解每個指令的執行時間以及安排執行順序非常重要。

- Big O
    - Introduction
        - Big O 是一個數學符號，在德文中表示 「Ordnung 」= 英文 「order of ...」= 中文 「 ... 階」，又稱為大 O 符號或漸進符號，用於描述函數漸進行為的漸進上限
        - 由德國數論學家 Paul Bachmann 和 Edmund Landau 發明的符號中的一個

    - O(1)
        - 表示時間複雜度是固定的，而不是處理數據的基數固定
        - O(1) 很重要，因為他無論基數或結構如何變化，複雜度都不會改變
        - O(1) 不表示執行兩次同樣的指令，執行時間也會相同，因為執行命令還需要經過 Cpu, Network ...etc 處理，當一個 Value 是 ABC 跟一個有 512 MB 的 Value 比較執行時間會有很大的差異
        - 不同的指令即使擁有相同的 O(1) 複雜度與處理相同的資料，其執行時間也會有差異
        - Example
            - APPEND, EXISTS, GET, SET, HGET, LPUSH, RPOP

    - O(N)
        - 大多數情況下處理資料的複雜度為 O(N)
        - 以 DEL 為例 ( 實際使用推薦 UNLINK )
            - 刪除一個 String data 時為 O(1)

                ```cmd
                MSET key:1 a key:2 b key:3 c
                DEL key:1
                ```

            - 刪除多個 String data 時為 O(N)
                - N 取決於刪除的資料量，下面的例子 N = 3

                    ```cmd
                    MSET key:1 a key:2 b key:3 c
                    DEL key:1 key:2 key:3
                    ```

            - 當刪除的資料中包含 List, Hash, Set, Sorted set 時為 O(M)
                - M 表示 List, Hash, Set, Sorted set 中元素的數量
                - 隨著集合大小的增加，複雜度也會隨之上升，而 DEL 會阻塞其他指令，這也是為什麼推薦使用 UNLINK 進行非阻塞操作
                - 以下範例 M = 3

                    ```cmd
                    SADD fruit apple banana orange
                    DEL fruit
                    ```

    - O(N * M)
        - SINTER 在多個 Set 間尋找交集，N 是較少的 Set 元素量，M 則是 Set 數量
        - 若要尋找兩個大 Set 間的交集，將因此得到更大的複雜度
        - 即使 Set 很小，但要尋找交集的 Set 很多，時間複雜度仍然很大，因為 N 和 M 之間是一個乘數
        - 以下的複雜度 N = 3, M = 2

            ```cmd
            SADD set-small a b c
            SADD set-large a d e f g h
            SINTER set-small set-large
            ```

    - O(S + N)
        - LRANGE 具有另一種形式的複雜度，S 為由左開始數多少個值，N 為取得的元素數量
        - 複雜度的增加取決於查詢的偏移量( Offset )與查詢的元素數量，即使是相當大的 List，若偏移量很小且只需取得少量元素，複雜度仍然會很低
        - 以下範例 S = 5 , N = 3 ( index 4~6 共三個元素 )

            ```cmd
            RPUSH my-list a b c d e f g h
            LRANGE my-list 4 6
            ```
        
- 使用 Reids 需要考慮以下幾點
    - Performance
        - Bit O notaion
        - Data cardinality ( 數據量 )
        - Data distribution ( 資料分佈 )
        - Time complexity != Wall clock time ( 執行時間 )

    - 計算時間複雜度，所有的複雜度定義都在 [redis.io](https://redis.io/) 上
    - 資料的基數，Set, Sorted set, List, Hash 的元素數量會影響整體的時間複雜度
    - 偏移量也可能是與複雜度相關的，尋找 List 中的第一個元素與中間的元素的複雜度會有很大的差異
    - 尋找 Hash 的 Field 複雜度始終是 O(1)，
    - 除了基數、數據結構和指令外還需考慮乘數，某些指令( e.g. SINTER )在達到一定資料量時複雜度會快速上升
    - 執行時間並非時間複雜度，始終要對 Production 的產品進行測試，以了解各種設計與指令選擇造成的影響
