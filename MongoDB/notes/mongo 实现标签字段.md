```markdown
{
    "_id" : ObjectId("4ee33229d8854784468cda7e"),
    "title" : "My Post",
    "content" : "This is a post with some tags",
    "tags" : [
        {
            "name" : "meta",
            "slug" : "34589734"
        },
        {
            "name" : "post",
            "slug" : "34asd97x"
        },
    ]
}
```

在查询包含 meta 标签时，使用下面的语句

You can then query for posts with a particular tag using *dot notation* like this:

```
db.test.find({ "tags.name" : "meta"})
```

Because `tags` is an array, mongo is clever enough to match the query against any element of the array rather than the array as a whole, and dot-notation allows you to match against a particular field.

在查询不包含 meta 标签时，使用下面的语句

To query for posts *not* containing a specific tag, use `$ne`:

```
db.test.find({ "tags.name" : { $ne : "fish" }})
```

在查询包含 meta 标签并且不包含 fish时，使用下面的语句

And to query for posts containing one tag but not the other, use `$and`:

```
db.test.find({ $and : [{ "tags.name" : { $ne : "fish"}}, {"tags.name" : "meta"}]})
```

Hope this helps!