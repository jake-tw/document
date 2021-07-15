## Hash

1. HSET key field value [field value ...]
    - 設定 Hash key 及屬性與屬性值
    - 可一次設定多個屬性
    - Example
        - HSET stationery pen 10 notebook 30

2. HMSET key field value [field value ...]
    - 同時設定多個 Field value
    - 較早期的版本 HSET 不可同時設定多個 Field value，需要使用 HMSET

2. HSETNX key field value
    - Field 不存在才執行操作
    - Key 不存在則建立一個新的 Hash 表

3. HGET key field
    - 取得指定 field 對應的 Value

4. HMGET key field [field ...]
    - 取得多個 fields 對應的 Value

5. HGETALL key
    - 返回所有 Field value，不建議在 Production 使用
    - 奇數行為 Field，偶數行為 Value，所以返回長度會是此 Hash 的兩倍

6. HKEYS key
    - 取得所有 Field，不建議在 Production 使用

7. HVALS key
    - 取得所有 Value，不建議在 Production 使用

6. HSCAN key cursor [MATCH pattern] [COUNT count]
    - 使用 Cursor 搜尋指定的 pattern
    - example HSCAN stationery 0 pen*

7. HEXISTS key field
    - 檢查 Field 是否存在

8. HDEL key field [field ...]
    - 刪除 Field

9. HINCRBY key field increment
    - 遞增指定 Field 的 Value ( 整數 )
    - Key or field 不存在時自動建立

10. HINCRBYFLOAT key field increment
    - 遞增指定 Field 的 Value ( 浮點數 )
    - Key or field 不存在時自動建立