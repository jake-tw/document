## Capped collections

- Check cardinality
    - List

        ```cmd
        LLEN key
        ```

    - Set

        ```cmd
        SCARD key
        ```

    - Sorted set

        ```cmd
        ZCARD key
        ```

- Use list
    - L/RPUSH + LTRIM
        - 使用 LTRIM 保留指定 Index 範圍內的成員
        - 時間複雜度取決於刪除的資料量，若大多數元素都會被保留的情況下，使用 LTRIM 會非常有效率

    - Use case
        - Activity stream ( e.g. FB 貼文 )
            - 只顯示最新五筆貼文

        - Init post

            ```cmd
            RPUSH post a b c d e
            ```

        - Push new post to the left
        
            ```cmd
            LPUSH post z
            ```

        - Trim list to retain top 5 post

            ```cmd
            LTRIM post 0 4
            ```
        
        - Check result

            ```cmd
            LRANGE post 0 -1
            ```

- Use sorted set
    - ZADD + ZREMRANGEBYRANK
        - 與 List 不同，ZREMRANGEBYRANK 的行為是刪除指定 Index 範圍的成員而非保留
        - 時間複雜度取決於成員數量，但當保留的成員大於刪除的成員時，這種方式仍然是有效率的

    - Use case
        - Leaderboard ( e.g. 遊戲積分榜 )
            - 只需要顯示分數最高的前三名

        - Init leaderboard

            ```cmd
            ZADD game 1 a 2 b 3 c 4 d 5 e
            ```

        - Add new highest-scoreing member

            ```cmd
            ZADD game 26 z
            ```

        - Retain top 5 score

            ```cmd
            ZREMRANGEBYRANK game 0 0
            ```

        - Check result

            ```cmd
            ZRANGE game 0 -1
            ```
