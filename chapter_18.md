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
* 视图缓存器--基于ByteBuffer
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
* 所有的xxxBuffer均是ByteBuffer上的一个视图而已，  
总是以操作ByteBuffer为目标，因为它直接与Channel交互。
* 基于ByteBuffer编码与解码  
```sh 
public static void codeError() throws UnsupportedEncodingException {
    ByteBuffer bb = ByteBuffer.wrap("我们还是原来的样子吗！".getBytes("utf-8"));
    CharBuffer cb = bb.asCharBuffer();
    System.out.println(Charset.forName("utf-8").decode(bb));//forName("ISO8859-1")
}
//output(decode:utf-8)
我们还是原来的样子吗！
//output(decode:ISO8859-1)
��bN��f/S�gev�h7[PT�
```
* XML序列化和反序列化对象  
* 序列化对象  
```sh 
import nu.xom.Document;
import nu.xom.Element;
import nu.xom.Serializer;

import java.io.BufferedOutputStream;
import java.io.FileOutputStream;
import java.io.OutputStream;
import java.io.UnsupportedEncodingException;
import java.util.Arrays;
import java.util.List;

/**
 * Created by Pengfei Jin on 2018/10/13.
 */
public class XMLSequence {
	public static void main(String[] args) throws Exception {
		List<Person> people = Arrays.asList(new Person("刘","玄德"),
				new Person("关","云长"),
				new Person("张","翼德"));
		System.out.println(people);
		Element root = new Element("people");
		for(Person p : people){
			root.appendChild(p.getXML());
		}
		Document doc = new Document(root);
		Person.format(System.out , doc);
		Person.format(new BufferedOutputStream(new FileOutputStream("D:\\people.xml")) , doc);
	}
}
class Person{
	private String first, last;
	public Person(String first, String last) {
		this.first = first;
		this.last = last;
	}
	public Element getXML(){
		Element person = new Element("person");
		Element firstName = new Element("first");
		firstName.appendChild(first);
		Element lastName = new Element("last");
		firstName.appendChild(last);
		person.appendChild(firstName);
		person.appendChild(lastName);
		return person;
	}
	public Person(Element person) {
		first = person.getFirstChildElement("first").getValue();
		last = person.getFirstChildElement("last").getValue();
	}
	@Override
	public String toString() {
		return first+" "+last;
	}
	public static void format(OutputStream os, Document doc) throws Exception {
		Serializer serializer = new Serializer(os , "utf-8");
		serializer.setIndent(4);
		serializer.setMaxLength(60);
		serializer.write(doc);
		serializer.flush();
	}
}
//output
[刘 玄德, 关 云长, 张 翼德]
<?xml version="1.0" encoding="utf-8"?>
<people>
    <person>
        <first>刘</first>
        <last>玄德</last>
    </person>
    <person>
        <first>关</first>
        <last>云长</last>
    </person>
    <person>
        <first>张</first>
        <last>翼德</last>
    </person>
</people>
```
* 反序列化对象（使用上述序列化实例代码）  
```sh 
class People extends ArrayList<Person>{
	public People(String filename) throws Exception {
		Document doc = new Builder().build(filename);
		Elements elements = doc.getRootElement().getChildElements();
		for (int i = 0; i < elements.size(); i++) {
			add(new Person(elements.get(i)));
		}
	}

	public static void main(String[] args) throws Exception {
		People p = new People("people.xml");
		System.out.println(p);
	}
}
//output
[刘 玄德, 关 云长, 张 翼德]
```