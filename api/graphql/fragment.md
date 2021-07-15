## Fragment

Fragment 是解決重複的複雜查詢的好幫手，使用起來相當簡單，就只是將某物件的查詢內容宣告為 Fragment，並用 '...fragmentName' 將 Fragment 插入指定的地方

```txt
{
    book(id: 1) {
        ...bookFields
    }
    author(id: 1) {
        ...authorFields
    }
}

fragment bookFields on Book {
    name
}

fragment authorFields on Author {
    id
    name
}
```

這樣的查詢相當於

```txt
{
    book(id: 1) {
        name
    }
    author(id: 1) {
        id
        name
    }
}
```

更複雜的查詢如下，可將 Fragment 插入多個相同 field 的位置

```txt
{
    leftComparison: hero(episode: EMPIRE) {
        ...comparisonFields
    }
    rightComparison: hero(episode: JEDI) {
        ...comparisonFields
    }
}
​
fragment comparisonFields on Character {
    name
    appearsIn
    friends {
        name
    }
}
```

