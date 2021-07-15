## Sorted Set

- ZADD KEY [NX | XX] [CH] [INCR score] score member [score member ...]
    - 加入一個包含 socre 的成員
    - NX
        - Key 不存在才執行操作
        - Equals : SETNX key value

    - XX
        - Key 已存在才執行操作

    - CH

    - INCR
        - 增加指定成員的分數
        - 使用 incr 只能指定一個成員
        - Equals : ZINCRBY key increment member

- Range
    - 查詢依照 Score 排序的 Set
    - 同 LRANGE，可指定是否要顯示 Score
    - Start 和 Stop 是 0 開始的 Index，不是 Score
        - ZRANGE key start stop [WITHSCORES]
            - Order by score asc

        - ZREVRANGE key start stop [WITHSCORES]
            - Order by score desc

- Range by score
    - 依照 Score 查詢範圍內的 Set
    - 最大值 : +inf, 最小值 : -inf
    - 預設數字都包含在查詢內，不包含使用符號 '('
        - ZRANGEBYSCORE key min max [WITHSCORES] [LIMIT offset count]
        - ZREVRANGEBYSCORE key min max [WITHSCORES] [LIMIT offset count]
    - Example
        - ZRANGEBYSCORE customer 0 100 WITHSCORES LIMIT 2 10
        - ZREVRANGEBYSCORE customer 100 0 WITHSCORES LIMIT 2 10

- Range by lex
    - 依照 Lex 查詢範圍內的 Set
    - 字串包含 '['，不包含 '('，不指定 '-'
        - 依照字典順序查詢
            - ZRANGEBYLEX key min max [LIMIT offset count]
            - ZREVRANGEBYLEX key min max [LIMIT offset count]

        - Example
            - ZRANGEBYLEX customer - [q LIMIT 2 10
            - ZREVRANGEBYLEX customer [z (a LIMIT 2 10

- Rank
    - 顯示成員的位置
    - Return zero based index
        - ZRANK key member
            - Order by score asc

        - ZREVRANK key member
            - Order by score desc

- ZSCORE key member
    - 顯示成員的 Score

- ZCOUNT key min max
    - 顯示分數區間內的成員數量
    - 等同於 ZRANGE 的成員數

- ZCARD key
    - 返回成員數量

- Remove
    - 刪除一個或多個成員
    - By value
        - ZREM member [member ...]

    - By lex
        - ZREMRANGEBYLEX key min max

    - By posittion
        - ZREMRANGEBYRANK key start stop
            - 刪除的成員包含 Start index 和 Stop index

    - By score
        - ZREMRANGEBYSCORE key min max

- ZINTERSTORE destination numkeys key [key ...] [WEIGHTS weight [weight ...]] [AGGREGATE SUM|MIN|MAX]
    - 允許對 Set 和 Sorted set 做操作
    - destination
        - 儲存排序後的結果的 Key ( 此操作會建立一個新的 Sorted set )

    - numkeys
        - 要操作的 Key 的數量

    - WEIGHTS
        - Score 會先乘上對應的 Weights 才進行後續操作
        - 可直接指定 Set 的 Score，Set score 預設 1

    - AGGREGATE
        - 指定如何計算新的 Score

- ZUNIONSTORE destination numkeys key [key ...] [WEIGHTS weight [weight ...]] [AGGREGATE SUM|MIN|MAX]
    - 同 ZINTERSTORE