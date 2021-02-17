# GraphQL vs RESTful

RESTful 跟 GraphQL 主要不同在於請求的部分，GraphQL 只有固定的進入點，而 RESTful 隨著功能增加與使用者需求，會發展出無數個進入點，並且 GraphQL 有固定的請求格式，RESTful 則需要依照 API 進行調整

||RESTful | GraphQL|
|-|-|-|
|進入點 | 無數個 ( GET /userId, POST /product...etc ) | 固定 ( POST /graphql, GET /graphql )|
|Request | URL / POST body...etc | Like a Graph|
|Response | Server 決定 | Client 決定|
|實作語言 | 任意 | 任意|
|狀態 | Stateless | Stateless|

使用 RESTful API 要找一本書的相關評論與作者資訊，可能會需要以下步驟

```txt
GET /books/:id
GET /authors/:id
GET /books/:id/comments
```

在 GraphQL 則只需要描述想要的資料結構即可

```txt
{
    books(id:bookId) {
        authors {
            name
        }
        comments
    }
}
```

當然在 RESTful 也能另外開更多的 API 來達成一次 Request 就查回所有資料的情境，但隨著時間經過，Legacy API 開始堆積後會越來越難以處理，而 GraphQL 只要簡單加上描述，就能增加「查詢同作者的其他著作」的需求

```txt
{
    book(id:bookId) {
        authors {
            name
            books {
                name
            }
        }
        comments
    }
}
```

而在 RESTful 查詢可能會有些冗餘的資料，像是最開始要查詢作者的 id 時，我們可能還不需要 price, bookType...等資訊，使用 GraphQL 的話每次都只取剛好需要的資料，不多不少。