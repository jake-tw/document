## String

1. SET key value [EX seconds | PX milliseconds] [NX | XX]
    - 設定 Key value pair
    - EX
        - 過期時間( 秒 )
        - Equals : SETEX key seconds value

    - PX
        - 過期時間( 毫秒 )
        - Equals : PSETEX key milliseconds value

    - NX
        - Key 不存在才執行操作
        - Equals : SETNX key value

    - XX
        - Key 已存在才執行操作
    
    - Example
        - SET customer:1
        - SET customer:1 EX 60
        - SET customer:1 PX 60
        - SET customer:1 EX 5 XX

2. MSET key value [key value ...]
    - 同時設置多個 key value pair

3. GET key
    - 取得對應的 Value
    - Time complexity: O(1)

4. DECRBY key decrement
    - 整數遞減，可為負數
    - Key 不存在時自動建立
    - 遞減 1 縮寫 : DECR key

5. INCRBY key increment
    - 整數遞增，可為負數
    - Key 不存在時自動建立
    - 遞增 1 縮寫 : INCR key

6. INCRBYFLOAT key increment
    - 浮點數操作，可為負數
    - Key 不存在時自動建立

7. APPEND key value
    - 如果是 String，將 Value 串在末尾
    - 如果不存在則創建一個

8. GETRANGE key start end
    - 返回 substring