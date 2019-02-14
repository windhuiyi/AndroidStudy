Gson 使用
=======

### Gson List 转换为 Json

```java
//list转换为json
Gson gson = new Gson();  
List<Person> persons = new ArrayList<Person>();  
String str = gson.toJson(persons);  

//json转换为list
Gson gson = new Gson();  
List<Person> persons = gson.fromJson(str, new TypeToken<List<Person>>(){}.
getType());  
```

### Gson @SerializedName 使用

- 可以将 json 返回的字段定义为自己想要的字段，或者将 json 返回的 java 用到的关键字解析为可以使用的字段，例如将 class 解析为 classX。
```java
    @SerializedName(value = "message", alternate = {"errorMsg"}) 
    private String msg; //alternate作用，将多个字段对应到一个，这样如果服务器将返回字段改变的时候可以用到。
    @SerializedName(value = "class")
    private String classX;
```