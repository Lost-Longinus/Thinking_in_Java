# Chapter 16 - ***Array***  
*  对象数组保存的是引用，默认初始化值为null；  
而基本类型数组则直接保存基本类型的值，默认初始化值为0，false等。   
* 多为数组，每一维用{}分隔  
```sh 
public class ArraySample {
	public static void main(String[] args) {
		int[][] a = {{1,2,3},{4,5,6}};
		System.out.println(Arrays.deepToString(a));
	}
}
//output  
[[1, 2, 3], [4, 5, 6]]
```  
* Arrays.fill()方法，只能用同一个值填充数组，作用十分有限：  
```sh 
public class ArraySample {
	public static void main(String[] args) {
		String[] strings = new String[6];
		Arrays.fill(strings,2,5,"Zhang");
		System.out.println(Arrays.toString(strings));
	}
}
//output 
[null, null, Zhang, Zhang, Zhang, null]
```  
* 数组排序  
> 方法一、实现Comparable接口  
> 方法二、Arrays.sort()方法中传入比较器Comparator  

