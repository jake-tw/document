## Type

1. Default type
    - Int : 32 bit 整數
    - Float : 雙精度浮點數
    - String : UTF-8 文字
    - Boolean : true, false
    - ID : ID 是特殊型別，唯一值，通常用於緩存，建議當成 String 看待

2. Scalar type
    - 自訂型別

        ```txt
        # 自訂 Date 型別
        scalar Date
        ```

3. Object type
    - 物件型別

        ```txt
            # User 物件
            type User {
                name
                height
            }
        ```

4. Enumeration type
    - 列舉，必填參數限制傳入值，或選填時給予預設值

        ```txt
        enum LengthUnit {
            "公尺"
            METER
            "公分"
            CENTIMETER
            "英尺"
            FOOT
        }

        type Starship {
            id: ID!
            name: String!

            "長度，預設公尺"
            length(unit: LengthUnit = METER): Float
        }
        ```

5. Array
    - 簡單加上 [] 即可

        ```txt
        type User {
            name
            [friend]
        }
        ```

6. Argument
    - 查詢參數

        ```txt
        type Query {        
            user(name: String): User
        }
        ```

7. Null
    - Nullable

        ```txt
        type Query {
            user(name: String): User
        }
        ```

    - Non-Null
        - 查詢參數

            ```txt
            type Query {
                user(id: ID!): User
            }
            ```
        - 回傳值

            ```txt
            type User {
                name: String!

                # 陣列不可 null，但裡面的值可以
                email: [String]!

                # 陣列可以 null，但裡面的值不可以
                phone: [String!]
            }
            ```

8. Root type
    - GraphQL 有三個 Root type，使用上跟 Object type 相同，特殊用途是當作整個系統的入口點

        ```txt
        schema {
            query: Query
            mutation: Mutation
            subscription: Subscription
        }
        ```
