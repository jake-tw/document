# Sorted Set

- 有順序的 Set
- 所有資料都只能用 String 來儲存，沒有嵌套概念，不能設定 list, set, sorted set, hash
- Sorting order by floating point score
    - 若分數一樣則比較 Lex order

- 依照 value, position, rank, lexicography 進行操作
- 支援集合操作，但只能將結果儲存到另一個 Sorted Set 中
    - Union
    - Intersection

- Use cases
    - Leaderboards
        - 加入排行榜

            ```cmd
            ZADD lb 90 a 100 b 3 c 60 d 22 e
            ```

        - 得分時增加分數

            ```cmd
            ZINCRBY lb 50 c
            ```

        - 查詢前三高分

            ```cmd
            ZREVRANGE lb 0 2 WITHSCORES
            ```

        - 保留前三名分數

            ```cmd
            ZREMRANGEBYRANK lb 0 -4
            ```

    - Priority quere
        - 加入任務

            ```cmd
            ZADD pq 1 p1 2 p2 3 p3 1 p1-2
            ```

        - 取出最優先的任務

            ```cmd
            ZRANGE pq 0 0
            ```

        - 刪除已完成任務

            ```cmd
            ZREM pq p1
            ```