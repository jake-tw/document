## Transaction

- Command
    - MULTI
        - Indicate the start of a transaction

    - EXEC
        - Execute the queued commands

    - DISCARD
        - Throws away any queued commands

    - WATCH key [key ...]
        - Declare interest in one or more keys

    - UNWATCH
        - Remove all watch keys

- Redis transaction
    - Redis transaction 不完全符合 ACID ( Atomicity 原子性、Consistency 一致性、Isolation 隔離性、Durability 持久性 )
    - Transaction 是一個單獨的隔離操作，操作過程中，其他用戶看不到資料的變化
    - 所有封裝在 Transaction 的命令，都會被序列化並依順序執行
    - 執行過程中不會被其他連線的命令打斷
    - 保證 Transaction 的原子性，要麼全部成功，要麼全部失敗
    - MULTI 後加入 Queue 的命令不會馬上執行，直到 EXEC 才會將這些命令做一個批次處理

- Differ from a transaction in a relational database
    - Programming errors
        - Syntax
            - 標記交易無效，EXEC 時不執行

        - Operation on incorrect data type
            - 加入 Queue，並在 EXEC 時報錯

    - System errors
        - Redis 具有保護措施，確保有任何寫入或不一致問題之前可以重啟 Redis process

    - Rollback
        - Redis 不具備 Rollback 功能，Redis 認為指令上的錯誤應該在開發階段處理，因為不需要支援 Rollback，所以 Redis 可以保持簡單且快速

    - Nested transaction
        - Other database
            - 指令立即執行
            - 消耗系統資源( Lock...etc )

        - Reids
            - 指令隊列
            - 單一操作中執行
            - 不支援 Nested transaction

- Optimistic concurrency control
    - 調用 MULTI 之前，使用 WATCH 監聽一或多個 Key
    - 如果監聽的 Key 在 MULTI 過程中發生改變，則 EXEC 放棄執行
    - 多個 WATCH 指令會累加，不會複寫之前已監聽的 Key
    - Local scope，所有命令都只在同一個 Client 端有效
    - EXEC 執行後，所有 WATCH 自動失效