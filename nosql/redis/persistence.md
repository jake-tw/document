# Redis persistence

- Persistence options
    - RDB ( Redis DataBase file | Redis Database Backup file )
        - Snapshot style persistence format
        - 預設的持久化策略
        - 按照指定的時間間隔蒐集數據的 Snapshots
        - 可自行保留數個 Snapshots，適合作為災難還原的備份檔
        - 檔案較小，備份頻率較低

    - AOF ( Append-Only File )
        - Change-log style persistent format
        - 紀錄 Redis server 的所有操作( 包含多餘的操作指令 )
        - Server 重啟時重現操作內容以還原數據
        - 檔案較大，還原時間較長
        - 不建議單獨使用 AOF

    - AOF + RDB
        - 如果希望獲得最完整的數據安全性，建議兩種持久化策略混合使用
        - 資料還原時優先使用 AOF 的數據，因為 AOF 的資料最完整

    - Disable
        - 若只打算將 Redis 用作 Cache，可以選擇關閉持久化

            ```txt
            // redis.conf
            appendonly no
            save ""
            ```

- Snapshotting
    - 在配置文件中設定執行間隔

        ```txt
        // 60 秒內有至少 1000 次 Key 變動時
        SAVE 60 1000
        ```
        
    - Fork 一份背景 process 進行資料備份
        - RDB 需要經常 Fork process 進行資料備份，如果資料太大且 CPU 性能不佳，可能會導致 Redis 延遲
        - AOF 也需要 Fork process，但可以調整 rewrite 的頻率

    - Parent process 只負責 Fork process 不進行 I/O 操作
    - Child process 開始寫入 Temp RDB file
    - 資料建立完成後，替換掉舊的文件( dump.rdb )
    - 利用 copy-on-write 機制
    - 預設執行間隔
        - save 900 1
        - save 300 10
        - save 60 10000

- Append-only file
    - 在配置文件中開啟設定

        ```txt
        appendonly yes
        ```

    - Log rewriting
        - 隨著時間經過 AOF 文件將越來越大
        - 當 AOF 文件大小超過設定時，Redis 會啟動重寫機制，只保留重建數據所需的最小指令
        - 與 RDB 一樣利用 copy-on-write 機制，並 Fork process 在背景執行
        - Parent process 會持續對現有的 AOF 文件進行寫入，以確保災難發生時，AOF 文件仍是準確的
        - 完成重寫後，替換掉舊的 AOF 文件

    - Fsync to Disk
        - Redis 支援三種 Fsync 設定
            - appendfsync always: 隨時
            - appendfsync everysec: 每秒一次( Default setting )
            - appendfsync no: 不同步

- Command
    - RDB
        - SAVE
            - Synchronous
            - 立刻執行資料備份

        - BGSAVE [SCHEDULE]
            - Asynchronous
            - Fork process 在背景執行
            - BGSAVE SCHEDULE 在 AOF rewrite 時會立刻返回 ok，避免衝突

        - LASTSAVE
            - 查詢上一次備份執行時間( UNIX time stamp )

    - AOF
        - BGREWRITEAOF
            - Asynchronous
            - Fork process 在背景執行
            - 對 AOF 進行重寫操作，Redis 2.4 後可透過設定自動觸發