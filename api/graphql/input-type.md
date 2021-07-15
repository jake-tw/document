## Input type

除了前面介紹的回傳型別與基本的傳入值以外，還有一種專門給複雜參數使用的 Input type

- Input type 限制
    - 不可有參數
    - 不可使用一般的 Object type( Output type )
    - 只能放入 Input type, Default type 當 Field
    
- Input type 除了關鍵字是 input 以外，和一般的 Object type( Output type ) 沒有區別

    ```txt
    input ReviewInput {
        stars: Int!
        commentary: String
    }
    ```

- 通常使用在 mutation
    - Query

        ```txt
        mutation CreateReviewForEpisode($review: ReviewInput!) {
            createReview(review: $review) {
                stars
                commentary
            }
        }
        ```

    - Variable

        ```json
        {
            "review": {
                "stars": 5,
                "commentary": "This is a great movie!"
            }
        }
        ```
