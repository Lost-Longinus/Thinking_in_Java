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
***读取文件内容时，换行符会被去掉，所以要恢复，须加上"\n"***   
 * 进程控制   
 ```sh 
 import java.io.BufferedReader;
 import java.io.IOException;
 import java.io.InputStreamReader;
 
 /**
  * Created by Pengfei Jin on 2018/10/13.
  */
 public class ProcessControl {
 	public static void main(String[] args) {
 	    //注意！！直接拷贝文件路径到IDEA，路径名前带上的“\u202A”会使路径无效，须删除
 		String cmd = "javap D:\\Demo2.class";
 		OSExecute.command(cmd);
 	}
 }
 class OSExecute{
 	public static void command(String cmd){
 		boolean err = false;
 		try {
 			Process process = new ProcessBuilder(cmd.split(" ")).start();
 			BufferedReader results = new BufferedReader(
 					new InputStreamReader(
 							process.getInputStream()
 					)
 			);
 			String s;
 			while ((s = results.readLine()) != null) {
 				System.out.println(s);
 			}
 			BufferedReader errors = new BufferedReader(
 					new InputStreamReader(
 							process.getErrorStream()
 					)
 			);
 			while ((s = results.readLine()) != null) {
 				System.out.println(s);
 				err = true;  
 			}
 		} catch (IOException e) {
 			if (!cmd.startsWith("CMD /C")){
 			    command("CMD /C" + cmd);
 			}else{
 				throw new RuntimeException(e);
 			}
 			if (err){
 			    throw new OSExecuteException("ERRORs executing" + cmd);
 			}
 		}
 	}
 }
 class OSExecuteException extends RuntimeException{
 	public OSExecuteException(String why){super(why);}
 }
 //output
 Compiled from "Demo2.java"
 public class cn.itcast.input.Demo2 {
   public cn.itcast.input.Demo2();
   public static void main(java.lang.String[]) throws java.io.IOException;
 }
 ```
 * 新I/O  
 接近于操作系统的执行方式：通道(FileChannel)和缓冲器(ByteBuffer)
 ![NEWIO](https://images0.cnblogs.com/blog2015/707997/201503/091339416439067.png)
```sh 
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.nio.channels.FileChannel;

/**
 * Created by Pengfei Jin on 2018/10/13.
 */
public class NewIO {
	public static void main(String[] args) throws IOException {
		FileChannel
				in = new FileInputStream("D:\\Github\\CodeSamples\\src\\DynamicProxy.java").getChannel(),
				out = new FileOutputStream("D:\\test.txt").getChannel();
		in.transferTo(0,in.size(),out);
		System.out.println(BufferedInputFile.read("D:\\test.txt"));
	}
}
//output
···
public class DynamicProxy {
	public static void main(String[] args) {
		SomeMethods proxy = (SomeMethods) Proxy.newProxyInstance(
				SomeMethods.class.getClassLoader(),
				new Class[]{SomeMethods.class},
				new InvocationHandler() {
···
```
* 视图缓存器--ByteBuffer
```sh 
public static void viewBuffer(){
    ByteBuffer bb = ByteBuffer.wrap(new byte[]{0,0,0,0,0,0,0,'a'});
    bb.order(ByteOrder.BIG_ENDIAN);//ByteOrder.LITTLE_ENDIAN
    IntBuffer ib = bb.asIntBuffer();
    while (ib.hasRemaining()) {
        System.out.println(ib.position()+" -> "+ib.get());
    }
}
//output(BIG_ENDIAN)
0 -> 0
1 -> 97
//output(LITTLE_ENDIAN)
0 -> 0
1 -> 1627389952
```
![bytebuffer](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1539429192745&di=c3c7eb7fd9d7502fe2b7026c8c6d29fd&imgtype=0&src=http%3A%2F%2Faliyunzixunbucket.oss-cn-beijing.aliyuncs.com%2Fjpg%2F7a79c310c18638545aeb775159735d48.jpg%3Fx-oss-process%3Dimage%2Fresize%2Cp_100%2Fauto-orient%2C1%2Fquality%2Cq_90%2Fformat%2Cjpg%2Fwatermark%2Cimage_eXVuY2VzaGk%3D%2Ct_100)  


