## Set

1. SADD key member [member ...]
    - 加入新成員

2. SMEMBERS key
    - 取得所有成員，不建議在 Production 使用

3. SCARD key
    - 返回成員數量

4. SSCAN key cursor [MATCH pattern] [COUNT count]
    - 使用 Cursor 搜尋指定的 pattern
    - Time complexity: O(N)
    - Example
        - SSCAN customer 0 MATCH *

5. SISMEMBER key member
    - 檢查成員是否存在

6. SREM key member [member ...]
    - 指定刪除某成員

7. SPOP key [count]
    - 刪除並返回一個隨機成員

8. 比較集合
    - 受到操作的 Set 基數影響，應該避免在龐大的集合中使用
        - SDIFF key [key ...]
            - 返回最左邊的 Key 的成員與其他 Key 的成員不重複的部分，並非差集
            - Equals : SDIFFSTORE ( 會儲存在新 Key 中 )

        - SINTER key [key ...]
            - 取得所有成員重疊的部分
            - 避免在龐大的集合中使用
            - Time complexity: O( N * M )
                - 複雜度取決於最小的 Set 大小( N )與 Set 的數量( M )

            - Equals : SINTERSTORE ( 會儲存在新 Key 中 )

        - SUNION key [key ...]
            - 取得所有不重複的成員
            - 避免在龐大的集合中使用
            - Equals : SUNIONSTORE ( 會儲存在新 Key 中 )