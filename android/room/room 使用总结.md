Room 使用总结
=========
### Room 要求列到值唯一
- 在`Entity`添加`indices`,并且设置`unique`为true

```java
@Entity(indices = {@Index(value = {"first_name", "last_name"},
            unique = true)})
    class User {
        @PrimaryKey
        public int id;

        @ColumnInfo(name = "first_name")
        public String firstName;

        @ColumnInfo(name = "last_name")
        public String lastName;

        @Ignore
        Bitmap picture;
    }
```

### Room 查找 符合条件的个数

```java
@Query("SELECT COUNT(is_checked) FROM table WHERE is_checked = 1")
public abstract int  getNumberOfRows();
```
