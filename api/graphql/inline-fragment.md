## Inline fragment

GraphQL 與大多數語言一樣都有 interface 和 union type 的概念，可以使用 Inline fragment 限制回傳的物件類型，或依照型別回傳不同種類的資訊

```txt
{
    search(text: "an") {
        __typename
        ... on Human {
            name
        }
        # ... on Droid {
        #     name
        # }
        ... on Starship {
            name
            model
        }
    }
}
```

觀察回傳值可以發現

1. Droid 註解掉了，所以不會出現
2. Starship 會多取得 model 的資料

```txt
{
    "data": {
        "search": [
            {
                "__typename": "Human",
                "name": "Han Solo"
            },
            {
                "__typename": "Starship",
                "name": "TIE Advanced x1"
                "model": "Twin Ion Engine Advanced x1"
            }
        ]
    }
}
```
