# Directive

Directive 需要搭配前面學到的 Variable 使用，用於調整 Query 與 Schema 的行為

- Client 端使用稱為 Executable Directive 或 Query Directive
    -  原生支援的 Directive 有兩個，@include, @skip 為 Query 增加條件判斷

        ```
        # Boolean 記得加上驚嘆號
        query TestDirective($skip: Boolean!, $include: Boolean!) {
            allBooks @skip(if: $skip){
                name
            }
            allAuthors @include(if: $include) {
                name
            }
        }
        ```

        ```
        // Variables
        {
            "skip": false,
            "include": true
        }
        ```

- Server 端使用稱為 Type System Directive 或 Schema Directive
    - 原生支援 @deprecated，廢除 Field 又避免 Breaking change

        ```txt
        type Book {
            id: ID!
            name: String
            authorId: ID! @deprecated(reason: "test")
            "reason 可加省略"
            author: Author @deprecated
        }
        ```

- 除此之外還能自訂 Directive 為 Schema 加上如：參數檢查、權限檢查...等功能
    - Directive 可使用的範圍如下
        - ExecutableDirectiveLocation
            - QUERY
            - MUTATION
            - SUBSCRIPTION
            - FIELD
            - FRAGMENT_DEFINITION
            - FRAGMENT_SPREAD
            - INLINE_FRAGMENT
        - TypeSystemDirectiveLocation
            - SCHEMA
            - SCALAR
            - OBJECT
            - FIELD_DEFINITION
            - ARGUMENT_DEFINITION
            - INTERFACE
            - UNION
            - ENUM
            - ENUM_VALUE
            - INPUT_OBJECT
            - INPUT_FIELD_DEFINITION
    - example
        - 定義一個 Directive : example 並使用在 Query field 上

            ```txt
            # directive @example on FIELD

            fragment SomeFragment on SomeType {
                field @example
            }
            ```

        - 可以使用 '|' 表示要使用在多個地方

            ```txt
            directive @example on FIELD_DEFINITION | ARGUMENT_DEFINITION

            type SomeType {
                field(arg: Int @example): String @example
            }
            ```

- 對自訂的 Directive 有概念後，回頭看一下原生支援的三個 Directive 長什麼樣子
    - @skip

        ```txt
        directive @skip (if: Boolean!) on FIELD | FRAGMENT_SPREAD | INLINE_FRAGMENT
        ```

    - @include

        ```txt
        directive @include (if: Boolean!) on FIELD | FRAGMENT_SPREAD | INLINE_FRAGMENT
        ```

    - @deprecated

        ```txt
        directive @deprecated (
            reason: String = "No longer supported"
        ) on FIELD_DEFINITION | ENUM_VALUE
        ```
