## Interface

與多數語言相同，GraphQL 也支援使用 interface 的抽象設計，除了共通的部分之外還能引入獨有的屬性，回傳時禁止使用 Interface type，必須為 Interface 實作的 Object type

```txt
interface Character {
    id: ID!
    name: String!
    friends: [Character]
    appearsIn: [Episode]!
}

# 實作的物件必須有全部介面的屬性
type Human implements Character {
    id: ID!
    name: String!

    # 返回可能是 Human 或 Droid 類型的朋友
    friends: [Character]
    appearsIn: [Episode]!

    starships: [Starship]
    totalCredits: Int
}

type Droid implements Character {
    id: ID!
    name: String!
    friends: [Character]
    appearsIn: [Episode]!
    
    primaryFunction: String
}
```

使用 & 多重實作

```txt
type Business implements NamedEntity & ValuedEntity {
    name: String
    value: Int
    employeeCount: Int
}
```

查詢獨有的屬性時需要使用 Inline fragment

```txt
query HeroForEpisode($ep: Episode!) {
    hero(episode: $ep) {
        __typename
        name
        ... on Human {
            height
        }
        ... on Droid {
            primaryFunction
        }
    }
}
```

接下來介紹一個簡單的 Node Interface(*1)，除了標明重要物件之外，還可以方便 caching 及 batching

```txt
type Query {
    # 建議使用 Node 的 ID 不要重複
    node(id: ID!): Node
    nodes(ids: [ID!]): [Node]!
}

interface Node {
    # 這邊只要一個 ID
    id: ID!
}

type Book implements Node {
    id: ID!
    ...
}

type Author implements Node {
    id: ID!
    ...
}
```

Node 查詢範例

```txt
{
    fourNode: node(id: "4") {
        id
        ... on User {
            name
        }
    }
    fiveNode: node(id: "5") {
        id
        ... on User {
            name
        }
    }
}
```

> 1. [Github](https://developer.github.com/v4/), [Shopify](https://shopify.dev/docs/storefront-api/reference) ...等大型公司都有實作 Node interface