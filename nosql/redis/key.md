## Keys

- Unique
- Binary safe
    - Redis 可使用任意字元當作 Key 值

- 可使用空格，但 KEY 必須是帶有雙引號的字符串
    - e.g. set "this is a key"

- Up to 512 MB
    - Redis 支援超長的 Key 值，且未來可能會再增加，建議在長度與可讀性取得平衡

- 不保留空值的 Key
    - 當資料結構為 Hash, List, Set, Sorted Set 時，若將所有的值刪除，使用 EXISTS 查詢會返回 0 ( 不存在 )

- Logical database
    - 一個 Redis 可以有多個邏輯數據庫
    - 不同邏輯數據庫的 Key 值可以重複，可在單一 Application 中分離 Key 值，而不用拆成多個 Application
    - Redis 沒有傳統數據庫的概念，所有資料都存放在同一個 RDB / AOF 文件中
    - Redis 希望所有事情都是很簡單的，避免了 Namespace 管理的複雜性
    - database index 由 0 開始，預設使用 database 0
    - Redis cluster 只支援 database 0
    - 大部分工具與框架都假設使用 database 0

- Name
    - user:id:follows
        - user:1000:follows
            - user : object name
            - 1000 : unique identity of the instance
            - follows : composed object
    
    - 上面這是 Redis 社群常見的命名方式，實際使用依照團隊約定好的一致性命名即可
    - 區分大小寫
        - user:1000:follows 與 User:1000:Follows 是不同的

- Expiration
    - Redis 會將資料保存在內存中直到空間用完為止
    - 可設定 TTL ( time to live ) 指定過期時間
    - TTL 可自由變更

- Plymorphism
    - 同一個 Key 可以放入不同資料型別的 Value