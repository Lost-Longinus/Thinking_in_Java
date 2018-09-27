### Chapter 13 - ***String***
* String对+和+=方法重载，实际上仍然调用了StringBuilder的append方法
所以，当字符串连接数量少时，直接用+法操作方便。但在循环体中，使用StringBuilder会提高效率。
