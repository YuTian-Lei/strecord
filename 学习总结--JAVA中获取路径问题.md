## Servlet获取路径的方法

```
//工程目录 - D:\\Users\\lenovo\\IdeaProjects\\mmall\\WebManage\\target\\mmall\\
request.getSession().getServletContext().getRealPath("");
//application路径 - /WebManage_war
request.getSession().getServletContext().getContextPath();
//application路径下的资源路径 
request.getSession().getServletContext().getResourcePaths("/");
//application路径 - /WebManage_war
request.getContextPath();
//当前servlet的路径 - /test/path
request.getServletPath();
```

## 类中获取路径的方法

```
//类中取得资源文件路径
URL classLoaderCurrentClassPath = TestController.class.getClassLoader().getResource("zfbinfo.properties");
```

## 文件对象获取路径的方法

```
File file = new File("test.txt");
//D:\\apache-tomcat-8.5.15\\apache-tomcat-8.5.15\\bin\\test.txt
String canonicalPath = file.getCanonicalPath();
//D:\\apache-tomcat-8.5.15\\apache-tomcat-8.5.15\\bin\\test.txt
String absolutePath = file.getAbsolutePath();
//test.txt
String filePath = file.getPath();
```

##  写入文件的用法

```
//创建File对象，但是还没有创建文件
File file = new File("D:\\ojbk\\test.txt");
if(!FileUtil.exist(file)){
   //创建文件目录
   FileUtil.mkParentDirs(file);
   //创建文件
   file.createNewFile();
 }
 //获取文件的输出流
 OutputStream out = new FileOutputStream(file);
 out.close();
 FileUtil.del(file);
```

## 读入文件的用法

```
//打成war或者jar包后文件会出现找不到的问题，此种用法可以避免这种情况
InputStream in = this.getClass().getClassLoader().getResourceAsStream("zfbinfo.properties");
```