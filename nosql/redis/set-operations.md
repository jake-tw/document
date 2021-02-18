# Set operations

- 尋找最有價值的客戶
    - Init customer
        - 老客戶

            ```cmd
            ZADD prod:1 200 Bill 50 Mary 80 June
            ZADD prod:2 800 Bill 1000 June 1600 Judy
            ```

        - 新商品潛在客戶

            ```cmd
            SADD prod:new:3 Bill Jason
            ```

    - 尋找買過所有種類商品的客戶

        ```cmd
        ZINTERSTORE report:1 2 prod:1 prod:2 AGGREGATE SUM
        ```

    - 若某些商品是主力銷售的，希望能有加權分數，並加入潛在客戶
        - 加入 Set 的情況，Score 可使用 Weights 直接指定

            ```cmd
            ZINTERSTORE report:1 3 prod:1 prod:2 new:prod:3 WEIGHTS 3 0.8 1000 AGGREGATE SUM
            ```

    - Check result

        ```cmd
        ZRANGE report:1 0 -1 WITHSCORES
        ```

- 門票販售
    - Init ticket

        ```cmd
        zadd ticket:capacity 1000 a 2000 b
        zadd ticket:sales -100 a
        ```

    - 檢查剩餘門票

        ```cmd
        ZUNIONSTORE ticket:availability 2 ticket:capacity ticket:sales AGGREGATE SUM
        ```

    - Check result

        ```cmd
        ZRANGE ticket:availability 0 -1 WITHSCORES
        ```