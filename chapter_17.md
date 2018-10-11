# Chapter 17 - ***Deep study of containers***   
![Container](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1539177980451&di=0521b3d885a6aca65b1910aa2f1db681&imgtype=0&src=http%3A%2F%2Fhi.csdn.net%2Fattachment%2F201012%2F14%2F0_1292308225m7x7.gif)  
* 集合类库完整图  
>collection中的可选操作
>>1、可选的方法就是该方法在父类中会抛出异常，如果子类不需要该方法就不必重写它，   
一但调用则抛出异常，如果需要就去重写它的功能。   
>>2、Collection中的 各种添加、 移除方法都是可选的。AbstractList ,   
AbstractSet,AbstractQueue中就是实现了可选功能，调用这些抽象类  
中的方法就会抛出异常。    
 
> 散列与散列码

>>在使用自己的类作为HashMap的键，必须重载hashCode()和equals()，  
默认的Object.equals()只是比较对象的地址。Object.hashCode()默认   
是使用对象的地址计算散列码。你必须同时重载hashCode()和equals()，  
HashMap使用equals()判断当前的键是否与表中存在的键相同。  

>>正确的equals()方法必须满足5个条件：   
（1）自反性。对任意x，x.equals(x)一定返回true。  
（2）对称性。对任意x和y，如果y.equals(x)返回true，则x.equals(y)一定返回true。  
（3）传递性。对任意x，y，z，如果有x.equals(y)返回true，y.equals(z)返回true，则x.equals(z)一定返回true。  
（4）一致性。对任意x和y，如果对象中用于等价比较的信息没有改变，那么无论调用   
     x.equals(y)多少次，返回的结果应该保持一致，要么一直是true要么一直是false。   
（5）对任何不是null的x，x.equals(null)一定返回false。    
  如果指定的数与参数相等返回0。     
  如果指定的数小于参数返回 -1。   
  如果指定的数大于参数返回 1  
* Hashtable、Vector、Stack是过去遗留下来的类，只是为了支持老程序，新程序中不要使用！！   
* 桶（bucket）  
单链表是为了解决哈希冲突，不同的key可能计算出相同的hash值，  
如果哈希值冲突了那么就用头插法将新值插入链表的头，作为数组中的元素，   
不用尾插法能省去了遍历链表的开销。
* 负载因子  
如默认0.75 当map的桶位数（键值对数量）达到 数组容量*0.75 时就会进行扩容 ，  
扩容后的数组是原来的2倍。

| 接口 | 实现 | 特性 |
| :---: | :---: | :---: |
| List  | ArrayList  | 相当于大小可变的数组，随机访问快，插入移除慢  |
| List  | LinkedList  | 插入移除快，随机访问慢，也实现了Queue接口  |
| set   | HashSet  | 元素无序，查询速度非常快  |
| set  | LinkedHashSet  | 按插入顺序保存，同时有HashSet的查询速度  |
| set  | TreeSet  | 按元素升序保存对象，或者根据元素实现的比较器排序 |
| Queue  | PriorityQueu  | 优先级高的先出，也实现了List接口 |
| Map  | HashMap   | 查找速度快，内部无规则排序 |
| Map  | LinkedHashMap   | 按插入顺序排序，查找速度快 |
| Map  | TreeMap  | 按升序保存对象，或者根据元素实现的比较器排序 |
