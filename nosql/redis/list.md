# List

- 依照插入順序排序的 String 集合
- 所有資料都只能用 String 來儲存，沒有嵌套概念，不能設定 list, set, sorted set, hash
- 可以有重複的值
- 沒有頭尾概念，對 Reids 來說，可選擇由左右任一側插入值
- 使用雙向鏈表，基於 Linked list 實作，提供了大量數據的性能解決方案
- 不建議頻繁的使用 Index 操作，可能會有效能上的問題
- 可使用在實現 Stack 或 Queue 功能
- User cases
    - Activity stream
        - 加入活動列表

            ```cmd
            LPUSH stream 1 2 3 4 5
            ```

        - 檢查活動

            ```cmd
            LRANGE stream 0 2
            ```

        - 保留最新的活動

            ```cmd
            LTRIM stream 0 3
            ```

    - producer-consumer pattern
        - Inter process communication
            - Produce

                ```cmd
                RPUSH queue "event1"
                ```
            - Consume

                ```cmd
                LPOP queue
                ```