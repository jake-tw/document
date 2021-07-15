## Union Type

Union type 與 Interface 非常相似，但查詢時不指定類型間的共同字段，用於不確定的回傳型別

- 宣告 Union type

    ```txt
    union SearchResult = Human | Droid | Starship
    ```

- 查詢

    ```txt
    query HeroForEpisode($ep: Episode!) {
        hero(episode: $ep) {
            __typename
            ... on Human {
                name
                height
            }
            ... on Droid {
                name
                primaryFunction
            }
        }
    }
    ```

- 與 interface 混合查詢

    ```txt
    {
        search(text: "an") {
            __typename

            # Human, Droid 會顯示 name
            ... on Character {
                name
            }
            ... on Human {
                height
            }
            ... on Droid {
                primaryFunction
            }

            # 沒有實作 Character，所以要額外查詢 name
            ... on Starship {
                name
                length
            }
        }
    }
    ```