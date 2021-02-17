# Introduction

- How fast is Redis?
    - 依照官方提供的資料，每秒請求數可超過 100000 以上
    - [benchmarks](https://redis.io/topics/benchmarks)

- Single thread
    - Redis 只有單執行緒，避免不必要的 Thread 切換
    - 不需考慮上鎖問題

- Data structure
    - Redis 的資料結構非常單純，只有 Key 加上 5 種資料類型
    - 沒有任何嵌套類，所有資料最終都是以 String 儲存

- In-memory
    - 絕大多數操作都是在 Memory 中進行，所以非常快速

- Epoll support
    - Redis 使用多路 I/O 復用模型

- Pipelining
    - 可同時發送多個指令，減少網路傳輸開銷