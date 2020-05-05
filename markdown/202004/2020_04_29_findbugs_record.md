# findbugs 记录

## 1. Possible double check of field

+ 双重加锁可能存在的一个问题就是：

  例如线程1已经分配了地址给instance 但是还没有初始化, 此时线程2 判断intance不是null 直接返回

  参考文档https://www.jianshu.com/p/78c6da609ccb

- 为啥要加双重锁：

  参考文档https://blog.csdn.net/u014253011/article/details/103916402

## 2. Incorrect lazy initialization and update of static field

该方法的初始化中包含了一个迟缓初始化的静态变量。你的方法引用了一个静态变量，估计是类静态变量，那么多线程调用这个方法时，你的变量就会面临线程安全的问题了，除非别的东西阻止任何其他线程访问存储对象从直到它完全被初始化。**结合1可以完美解决单例模式**

## 3. Method concatenates strings using + in a loop

```java
// This is bad
String s = "";
for (int i = 0; i < field.length; ++i) {
    s = s + field[i];
}

// This is better
StringBuffer buf = new StringBuffer();
for (int i = 0; i < field.length; ++i) {
    buf.append(field[i]);
}
String s = buf.toString();
```

## 4. Inefficient use of keySet iterator instead of entrySet iterator

参考文档：https://blog.csdn.net/weixin_38106322/article/details/101770477

如果需要获取value, keySet/entrySet/values

https://blog.csdn.net/u013776390/article/details/83106606

## 2.Boxed value is unboxed and then immediately reboxed

http://blog.sina.com.cn/s/blog_4adc4b090102vi9p.html