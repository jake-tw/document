# Key

1. KYES pattern
    - 搜尋 Redis 存在的 Key 值
    - 會 Block Reids 直到搜尋完成，如果有幾百萬個 Key 會造成整個 Redis 卡死一段時間
    - 建議不要在 Production 使用這個指令
    - 使用上比較簡單，通常用在 debug
    - 支援 [Glob-style pattern](https://en.wikipedia.org/wiki/Glob_(programming))
    - Example
        - KEYS customer:*
        - KEYS customer:1000

2. SCAN cursor [MATCH pattern] [COUNT count]
    - 使用 Cursor 查詢 Redis 存在的 Key 值
    - 也會 Block，但一次只回傳 0 到指定數量的 Key ( 通常一次一百個 )
    - Safe for production
    - Slot 放入查詢回傳的 Cursor ，重複查詢直到 Cursor 回到 0 結束 Iterate
    - Example
        - SCAN 0
        - SCAN 196608
        - SCAN 0 MATCH customer:*
        - SCAN 1286144 MATCH customer:*
        - SCAN 0 MATCH customer:* COUNT 1000

3. EXISTS key [key ...]
    - 檢查 Key 是否存在

4. DEL key [key ...]
    - Blocking operation
    - 直接刪除 Key

5. UNLINK key [key ...]
    - 刪除 Key 推薦使用 UNLINK
    - Asynchronous process, non-blocking command.
    - 取消 Key 關聯，由後台的 Thread 異步回收 Memory

6. TYPE key
    - 檢查 Key 型別

7. OBJECT ENCODING key
    - 檢查 Value 編碼

<br>
<br>
<br>

# TTL

1. Set
    - EXPIRE key seconds
        - 指定到期時間( 秒 )

    - EXPIREAT key timestamp
        - 指定到期時間戳( unix seconds timestamp )

    - PEXPIRE key milliseconds
        - 指定到期時間( 豪秒 )

    - PEXPIREAT key milliseconds-timestamp
        - 指定到期時間戳( unix milliseconds timestamp )

2. Inspect
    - PTTL Key
        - 檢查過期時間( 毫秒 )

    - TTL Key
        - 檢查過期時間( 秒 )

3. Remove
    - PERSIST Key
        - 刪除到期時間