java 汇总
========

### java  <<  左移运算符、 >> 右移运算符 、 >>> 无符号右移，忽略符号位

```java
    public void test() {
        int number = 123456789;
        //原始数二进制
        print(number);
        //左移一位  相当于乘以2
       int left = number << 4;
        print(left);
        //右移一位 相当于除以2
        int right = number >> 4 ;
        print(right);
        //无符号右移，忽略符号位，空位都以0补齐 无符号右移运算符>>> 只是对32位和64位的值有意义
        int notSingedRight = number >>> 4;
        print(notSingedRight);

    }

    private static void print(int num) {
        System.out.println(num);
        System.out.println(Integer.toBinaryString(num));
    }
```



### java 自动装箱、拆箱



#### 什么是自动装箱和拆箱

  > 自动装箱就是Java自动将原始类型值转换成对应的对象，比如将int的变量转换成Integer对象，这个过程叫做装箱，反之将Integer对象转换成int类型值，这个过程叫做拆箱。因为这里的装箱和拆箱是自动进行的非人为转换，所以就称作为自动装箱和拆箱。原始类型byte,short,char,int,long,float,double和boolean对应的封装类为Byte,Short,Character,Integer,Long,Float,Double,Boolean。

#### 自动装箱拆箱要点

- 自动装箱时编译器调用valueOf将原始类型值转换成对象，同时自动拆箱时，编译器通过调用类似intValue(),doubleValue()这类的方法将对象转换成原始类型值。
- 自动装箱是将boolean值转换成Boolean对象，byte值转换成Byte对象，char转换成Character对象，float值转换成Float对象，int转换成Integer，long转换成Long，short转换成Short，自动拆箱则是相反的操作。


