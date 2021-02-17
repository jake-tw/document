# Lua scripting

Redis 從 2.6 版本開始內嵌 Lua 執行環境，解決了之前不能高效率的處理 CAS ( check-and-set ) 命令的缺點，並通過組合使用多個命令，實現更多原先執行困難的 Pattern

- Command
    - EVAL script numkeys key [key ...] arg [arg ...]
        - Script : 單純的 Lua 程式，將在 Redis 中執行
        - Numkeys : Key 的數量
        - Key : 腳本中使用的 Redis key，通過 KEYS 指定 ( Base on index 1 )
        - Arg : 附加參數，通過 ARGV 指定( Base on index 1 )
        - Example

            ```cmd
            EVAL "return {KEYS[1], KEYS[2], ARGV[1]}" 2 key1 key2 arg1
            EVAL "return {'Constant1', 'Constant2'}" 0
            EVAL "return {'Constant1', KEYS[1], 'Constant2'}" 1 key1
            ```
    - EVALSHA sha numkeys key [key ...] arg [arg ...]
        - 同 EVAL，不過 Script 來源為 SCRIPT LOAD 緩存的腳本

    - SCRIPT FLUSH
        - 清除所有緩存的腳本

    - SCRIPT EXISTS sha1 [sha1 ...]
        - 檢查腳本是否存在

    - SCRIPT LOAD script
        - 將腳本載入緩存並返回該腳本的 sha

    - SCRIPT KILL
        - 終止當前運行的腳本

- Lua
    - 簡介
        - 使用標準 C 語言編寫的輕量手稿語言
        - 目標是成為一個很容易嵌入其它語言中使用的語言
        - 易於使用的擴展接口和機制，可由宿主語言提供額外的功能
        - 葡萄牙語中月亮的意思

    - Funcation
        - redis.call() : 發生錯誤時拋出 Lua error，並強制 EVAL 返回錯誤
            
            ```cmd
            SET foo bar
            EVAL 'redis.call("hget","foo","bar")' 0
            EVAL 'local t = redis.call("hget","foo","bar"); local r = {type(t)}; for k,v in pairs(t) do r[#r+1] = k; r[#r+1] = v; end; return r' 0
            ```

        - redis.pcall() : 發生錯誤時捕獲錯誤，並返回於 return 的 Table 中
            
            ```cmd
            SET foo bar
            EVAL 'redis.pcall("hget","foo","bar")' 0
            EVAL 'local t = redis.pcall("hget","foo","bar"); local r = {type(t)}; for k,v in pairs(t) do r[#r+1] = k; r[#r+1] = v; end; return r' 0
            ```

        - 建議使用符合 EVAL 的語意來傳遞 Key
            - 所有 Redis 命令在執行前都會被分析，以此來確定命令會對哪些 Key 進行操作，若要使分析工作在 EVAL 指令正確執行，必須使用正確的方式傳遞 Key
            - 確保請求可以發送到正確的 Redis cluster node
            - 非強制，但在 Redis single instance 下濫用的腳本將無法被 Redis cluster 兼容
            - Example

                ```cmd
                EVAL "return redis.call('set', 'foo', 'bar')" 0 // Not recommended
                EVAL "return redis.call('set', KEYS[1], 'bar')" 1 foo
                ```

    - Library
        - base, table, string, math, debug, cjson, cmsgpack
        - 除了 cjson 用於處理 JSON 外，都是標準的 Lua library

    - Data type mapping

        |Redis|Lua|
        |-|-|
        |integer|number|
        |bulk|string|
        |multi bulk|table<br>(may have other Redis data types nested)|
        |status|table with a single ***ok*** field containing the status|
        |error|table with a single ***err*** field containing the error|
        |Nil bulk reply and Nil multi bulk reply|false boolean type|
