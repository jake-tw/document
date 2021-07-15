## GraphQL Introduction

GraphQL 是一種為了 API 設計的查詢語言，提供完整且易於理解的資料結構描述，Client 端有權主動決定返回的資料，讓開發變得更具彈性。

此項技術是由 Facebook 在 2012 年，為了移動端以及 News Feed 功能而開發，並在 2015 年開源，使用上簡單來說分為以下三個步驟：

1. Server 定義資料結構

    ```txt
    type Project {
        name: String
        tagline: String
        contributors: [User]
    }
    ```
    
2. Client 描述想要查詢的資料

    ```txt
    {
        project(name: "GraphQL") {
            tagline
        }
    }
    ```

3. Client 取得可預期的結果

    ```json
    {
        "project": {
            "tagline": "A query language for APIs"
        }
    }
    ```
