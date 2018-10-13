# Chapter 18 - ***Java I/O System***  
*   InputStream/OutputStream面向***字节***形式的I/O；  
Reader/Writer则面向***字符***形式的I/O；  
InputStreamReader/OutputStreamWriter实现了字节向字符的转换。   
![I/O](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1539409667516&di=18ce4833ac01b021231cb790bb562d4a&imgtype=0&src=http%3A%2F%2Fimage.mamicode.com%2Finfo%2F201510%2F20180110170051259390.jpg)  
* 输入输出实例
 ```sh 
 import java.io.*;
 
 /**
  * Created by Pengfei Jin on 2018/10/13.
  */
 public class InOutPut {
 	static String file = "D:\\JdbcUtil_copy.java";
 	public static void main(String[] args) throws IOException {
 		BufferedReader in = new BufferedReader(
 				new StringReader(
 						BufferedInputFile.read("D:\\JdbcUtil.java")
 				)
 		);
 		PrintWriter out = new PrintWriter(file);
 		int lineCount = 1;
 		String s;
 		while ((s = in.readLine()) != null) {
 			out.println(lineCount++ + " : " + s);
 		}
 		out.close();
 		System.out.println(BufferedInputFile.read(file));
 	}
 }
 class BufferedInputFile{
 	public static String read(String filename) throws IOException {
 		BufferedReader in = new BufferedReader(new FileReader(filename));
 		String s;
 		StringBuilder sb = new StringBuilder();
 		while ((s = in.readLine()) != null) {
 			sb.append(s + "\n");
 		}
 			in.close();
 		return sb.toString();
 	}
 }
 //output  
···
26 : 	public static Connection getConnection() {
27 : 		try {
28 : 			Class.forName("com.mysql.jdbc.Driver");
29 : 			return DriverManager.getConnection(url, user, password);
30 : 		} catch (Exception e) {
31 : 			throw new RuntimeException(e);
32 : 		}
···
```  

