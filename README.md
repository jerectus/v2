# sql
## DDL
```
create table BOOK (
  ID number primary key,
  TITLE varchar,
  AUTHOR varchar
)
```

## Model
```
public class Book {
  @Id
  public int id;
  public String title;
  public String author;
}
```

## Db
```
try (var db = Db.open()) {
  ...
}
```

## select
```
db.select(Book.class).forEach(book -> {
  System.out.println("title=" + book.title);
});
// select * from BOOK
```

## select ... where ... order by
```
db.select(Book.class, x->x
  .where("TITLE like ?", title)
  .orderBy("TITLE")
).forEach(book -> {
  System.out.println("title=" + book.title);
});
// select * from BOOK where TITLE like ? order by TITLE
```

## select by Primary Key
```
var book = db.select(Book.class).byId(10);
System.out.println("title=" + book.title);
```

## insert - DSL
```
db.insert(Book.class).with(x->x
  .set("ID", "VALUE1")
  .set("TITLE", "VALUE2")
);
// insert into BOOK (ID, TITLE) values (?, ?)
```

## insert - Object
```
var book = new Book();
book.id = 1;
book.title = "ONE";
db.insert(book);
// insert into BOOK (ID, TITLE) values (?, ?)
```

## update - DSL
```
db.update(Book.class).with(x->x
  .set("TITLE", "TWO")
  .where("ID = ?", 2)
);
// update BOOK set TITLE = ? where ID = ?
```

## update - Object
```
var book = db.select(Book.class).byId(1);

book.title = "THREE";
db.update(book)
// update BOOK set TITLE = ?, AUTHOR = ? where ID = ?

book.title = "FOUR";
db.update(book, "TITLE")
// update BOOK set TITLE = ? where ID = ?

book.author = "Mitoma";
db.update(book, "!TITLE")
// update BOOK set AUTHOR = ? where ID = ?
```

## update - Object 2
```
var book = db.select(Book.class).byId(1);

db.update(book, it -> {
  it.title = "FIVE";
);
// update BOOK set TITLE = ?, where ID = ?

db.update(book, it -> {
  it.author = "Minamino";
);
// update BOOK set AUTHOR = ? where ID = ?
```

## delete - DSL
```
db.delete(Book.class).with(x->x
  .where("ID = ?", 123)
);
// delete from BOOK where ID = ?
```

