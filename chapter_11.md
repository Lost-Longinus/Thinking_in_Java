### Chapter 11 - ***Hold Object***
* 对象容器
![image](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1537876138591&di=e09528a5920737e4709466e5c81d02a3&imgtype=0&src=http%3A%2F%2Fimages.cnblogs.com%2Fcnblogs_com%2Ftianyake%2F201202%2F201202291522521809.jpg)
* Collection--每个槽保存一个对象
 1. List--以特定的顺序保存一组元素
 2. Set--元素不能重复  
 3. Queue--只允许在容器的一端插入对象，并从另一端移除对象
 * Map--每个槽保存一个键/值对
 ----
 * ArrayList/LinkedList排列顺序即插入顺序
* 无论Collection或是Map,有实现类前缀有以下特征  

| 前缀名 | 排序特征 |  
| :---: | :---: |  
| Hash- | 通过Hash表顺序排列 |  
| Tree- | 按照比较器排列顺序 |  
| LinkedHash- | 按照插入顺序排列 |  
* 关于堆栈的概念解释很多，但各种说法不统一，很容易让人混淆。
* 这里有必要从中文含义上，明晰概念。
>Stack(由LinkedList提供支持)
> >堆>>草堆，堆碟子>>后进先出  
![image](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1537883283921&di=193b803140bfbc168c8691cc38a1a91a&imgtype=0&src=http%3A%2F%2Fimg.it610.com%2Fimage%2Finfo3%2F7b27ffab6d9f4c87ab0947049a4bd4cc.png)

>Queue(由LinkedList提供支持)
>>栈/队列>>栈道>>由于道路狭窄，无法掉头>>先进先出  
![image](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1537883376027&di=6da4b1246f55031a798c7d69f5ed5e07&imgtype=0&src=http%3A%2F%2Fimgsrc.baidu.com%2Fimgad%2Fpic%2Fitem%2F3b87e950352ac65c1efe1beef1f2b21192138ab7.jpg)
特殊队列--优先级队列PriorityQueue
```sh 
public class HoldObject {
	public static void main(String[] args) {
		PriorityQueue<Integer> priorityQueue = new PriorityQueue<Integer>();
		Random random = new Random(47);
		for (int i = 0; i <16 ; i++) {
			priorityQueue.add(random.nextInt(20));
		}
		System.out.println(priorityQueue);
	}
}
//output 
[0, 1, 8, 1, 7, 8, 9, 18, 2, 13, 8, 15, 11, 9, 9, 18]
//使用比较器，按优先级高低排序
```

**Map的强大之处**
Map的值可以是其它容器，包括Map/List
* Java SE5 新特性--Iterable接口  
所有Collection实现了该接口，其iterator()方法返回Iterator,  
该接口的实现类可用于foreach遍历。
 