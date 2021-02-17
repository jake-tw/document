# String

- Text data
- Integer and Floating Point number
    - Redis 對數字操作順序如下
        1. 接收 INCR 之類數字操作指令
        2. 檢查 Key 型別是否正確
        3. 檢查 Value encoding
        4. 改變 value
        
- Operation by value type
    - Strings: APPEND, GETRANGE, SETRANGE, STRLEN
    - Integers: INCR, INCRBY, DECR, DECRBY
    - Floats: INCRBYFLOAT
    - Bits: SETBIT, GETBIT, BITPOS, BITCOUNT, BITOP

- Binary data
- Maximum of 512 MB
- 不管是哪種狀態儲存，都是用 String data 儲存