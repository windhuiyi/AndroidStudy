BitmapFactory 使用总结
==================

### BitmapFactory 使用

#### BitmapFactory 作用

> Creates Bitmap objects from various sources, including files, streams, and byte-arrays.

##### `decodeByteArray`

> 从网络上获得的图片数据为Base64编码格式的字符串，使用BitmapFactory将其转换成Bitmap对象

```java
    private Bitmap stringtoBitmap(String string){
        
        Bitmap bitmap=null;
        
        try {
        	byte[] bitmapArray;
        	bitmapArray = Base64.decode(string, Base64.DEFAULT);
        	bitmap = BitmapFactory.decodeByteArray(bitmapArray, 0, bitmapArray.length);
        } catch (Exception e) {
        	e.printStackTrace();
        }
       
        return bitmap;
    }
```

#####  `decodeResource`

> 从resource资源文件解码将其转换成Bitmap对象，比如放在drawable或者mipmap目录下的图片。

```java
    Bitmap bitmap = BitmapFactory.decodeResource(context.getResources(),drawable);
```

##### 需要注意内存大小，避免OOM
    (1) 可以缩小尺寸，通过`BitmapFactory.Options`的`inSampleSize`属性。值必须大雨1，2为缩小2倍，即宽高是原来的1/2，像素是原来的1/4。

    ```java
        BitmapFactory.Options bmpFactoryOptions = new BitmapFactory.Options();    
        bmpFactoryOptions.inSampleSize = 2;   
    ```
    (2) 使用完记得`recycle`
    ```java
        if(!bitmap.isRecycled()){  
            bitmap.recycle(); //回收图片所占的内存  
            System.gc(); //提醒系统及时回收  
        }  
    ```

###### 参考文章
- [原Android BitmapFactory用法总结](https://blog.csdn.net/lindonghai/article/details/49618835)


### 获取图片大小

```java
BitmapFactory.Options options = new BitmapFactory.Options();
options.inJustDecodeBounds = true;
Bitmap bmp = BitmapFactory.decodeFile(path, options);
//options.outWidth  和 options.outHeight就是我们想要的宽和高
```