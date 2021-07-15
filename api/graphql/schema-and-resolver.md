## Schema and Resolver

GraphQL API 最重要的兩個元件就是 Schema 和 Resolver

- Schema : 定義 GraphQL API 的資料架構與提供文件描述
- Resolver : 負責取得資料的行為，類似 RESTful 中的 Controller

打個比方

- 餐廳有菜單，上面列出所有客戶需要的資訊，包含價格、品項...等，當客戶可以自由組合餐點內容，點完餐後，店員接收訂單並準備出餐
- 系統有 Schema，上面列出所有 Client 可取得的資訊，包含價格、品項...等，當 Client 發送 Query 後，Resolver 負責解析 Query 並取得資料

GraphQL schema 是由數個 Type 與 Type 之間的關聯所組成，若使用 GraphQL Voyager(*1) 可以將 Schema 解析為一幅視圖，所有的元素都由 Root 出發，並根據 Type 的定義逐步往下查找，下面來介紹 Schema 的基本用法

1. 一定要有 Root type
2. 使用 type 宣告
3. 強型別
4. 使用 ! 表示必填
5. 使用 " 或 # 加上註解

```txt
"""
多行註解
會在文件中呈現
"""
# 一般註解
type Query {
    "單行註解，會在文件中呈現"
    book(id: ID!): Book
    allBooks: [Book]
    countBooks: Int
}

type Book {
    id: ID!
    name: String
}
```

> 1. https://apis.guru/graphql-voyager/