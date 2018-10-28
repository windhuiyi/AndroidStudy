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