# JNDI

**1、JNDI入门**

 JNDI（Java Naming and Directory Interface，Java 命名和目录接口）是一组在Java应用中访问命名服务和目录服务的API。其中，JavaEE要求Web容器（如：tomcat）必须实现JNDI规范。

 怎么理解这一项技术呢？我们可以给每个资源起一个名字，并且构建一成个目录结构，就好比linux系统当中的目录结构一样，这样我们就可以像访问文件这样/usr/local/config/web.xml，去访问一个资源，这个资源可以是任意我们可以用java定义的资源，比如我们的数据源。

 资源引用和资源定义的默认 JNDI 命名空间必须始终是**java:comp/env**，这就好比一个默认的文件夹。

 看这个图，我们怎么表示一个mysql的数据源呢？

​    ![0](data:image/png;base64,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)

java:comp/env/dataSource/mysql 这样是不是就行呢？

**2、JNDI应用：配置数据源**

1）在tomcat中新增命名服务

第一步：向tomcat安装目录下的lib中添加JDBC驱动程序

第二步：修改tomcat中config目录下的context.xml

```
<Context>
​
    <!-- Default set of monitored resources. If one of these changes, the    -->
    <!-- web application will be reloaded.                                   -->
    <WatchedResource>WEB-INF/web.xml</WatchedResource>
    <WatchedResource>WEB-INF/tomcat-web.xml</WatchedResource>
    <WatchedResource>${catalina.base}/conf/web.xml</WatchedResource>
​
    <!-- Uncomment this to enable session persistence across Tomcat restarts -->
    <!--
    <Manager pathname="SESSIONS.ser" />
    
    -->
    <Resource name="dataSource/mysql/prod"
              auth="Container"
              type="javax.sql.DataSource"
              driverClassName="com.mysql.cj.jdbc.Driver"
              url="jdbc:mysql://127.0.0.1:3306/ydlclass?characterEncoding=utf8&amp;serverTimezone=Asia/Shanghai"
              username="root" password="root"
              maxTotal="20" maxIdle="10"
              maxWaitMillis="10000" />
​
    <Resource name="dataSource/mysql/test"
              auth="Container"
              type="javax.sql.DataSource"
              driverClassName="com.mysql.cj.jdbc.Driver"
              url="jdbc:mysql://127.0.0.1:3306/boke?characterEncoding=utf8&amp;serverTimezone=Asia/Shanghai"
              username="root" password="root"
              maxTotal="20" maxIdle="10"
              maxWaitMillis="10000" />
</Context>
```



第三步，在代码中访问

```
Context ctx = null;
try {
    ctx = new InitialContext();
    DataSource dataSource = (DataSource)ctx.lookup("java:comp/env/dataSource/mysql");
    System.out.println(dataSource);
} catch (Exception e) {
    e.printStackTrace();
}
```



2）在当前工程下新增命名服务

第一步：向WEB-INF/lib目录下添加mysql驱动程序

第二步：在与WEB-INf同级的目录下新建META-INF/context.xml并配置

```
<?xml version="1.0" encoding="UTF-8"?>
<Context>
    <Resource name="dataSource/mysql/prod"
              auth="Container"
              type="javax.sql.DataSource"
              driverClassName="com.mysql.cj.jdbc.Driver"
              url="jdbc:mysql://127.0.0.1:3306/ydlclass?characterEncoding=utf8&amp;serverTimezone=Asia/Shanghai"
              username="root" password="root"
              maxTotal="20" maxIdle="10"
              maxWaitMillis="10000" />
​
    <Resource name="dataSource/mysql/test"
              auth="Container"
              type="javax.sql.DataSource"
              driverClassName="com.mysql.cj.jdbc.Driver"
              url="jdbc:mysql://127.0.0.1:3306/boke?characterEncoding=utf8&amp;serverTimezone=Asia/Shanghai"
              username="root" password="root"
              maxTotal="20" maxIdle="10"
              maxWaitMillis="10000" />
​
</Context>
```



3）基础数据类型

```
<env-entry>
    <env-entry-name>baseUrl</env-entry-name>
    <env-entry-type>java.lang.String</env-entry-type>
    <env-entry-value>D://www/</env-entry-value>
</env-entry>
```

```
Context ctx = null;
try {
    ctx = new InitialContext();
    DataSource dataSource = (DataSource)ctx.lookup("java:comp/env/baseUrl");
    System.out.println(dataSource);
} catch (Exception e) {
    e.printStackTrace();
}
```



**3、使用JNDI好处**

 以JNDI配置数据源为例，当数据源变更（如：更换数据库类型，更改用户名或密码，更改连接的URL等），只需要web服务器管理员去修改JNDI数据源的配置文件即可，不需要开发人员去修改程序代码，从一定程度上达到了程序解耦的目的。同时，不仅是数据源如此，对于程序使用其他外部资源的情况，也可以使用JNDI配置.

**4、@Resource**

```
@WebServlet("/")
public class MyServlet extends HttpServlet {
​
    @Resource(lookup="java:comp/env/baseUrl")
    DataSource dataSource;
​
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println(dataSource);
    }
}
```



# Lambda表达式

**Lambda 表达式**，也可称为闭包，它是推动 Java 8 发布的最重要新特性。

Lambda 允许把函数作为一个方法的参数（函数作为参数传递进方法中）。

使用 Lambda 表达式可以使代码变的更加简洁紧凑。

**函数式编程**思想概述 函数式思想则尽量忽略面向对象的复杂语法：“强调做什么，而不是以什么形式去做” 而我们要学习的Lambda表达式就是函数式思想的体现

格式： (形式参数) -> {代码块} 形式参数：如果有多个参数，参数之间用逗号隔开；如果没有参数，留空即可 ->：由英文中画线和大于符号组成，固定写法。代表指向动作 代码块：是我们具体要做的事情，也就是以前我们写的方法体内容 组成Lambda表达式的三要素： 形式参数，箭头，代码块

Lambda表达式的使用前提 有一个接口 接口中有且仅有一个抽象方法

Lambda表达式的省略模式【应用】 省略的规则 1参数类型可以省略。2但是有多个参数的情况下，不能只省略一个 如果参数有且仅有一个，那么小括号可以省略3 如果代码块的语句只有一条，可以省略大括号和分号，和return关键字。

**具体类使用匿名内部类**

 ```
 public class Student {
 public void study() {
     System. out. printin("爱生活，爱Java");
 }
 public class LambdaDemo {
 public static void main(String[] args) {useStudent(new Student(){
 @Override
 public void study() {
 System. out . print1n("具体类");
 }
 });
 }
 private static void useStudent(Student s) {
 s. study();
 }
 }
 ```

​            

7Lambda表达式的注意事项【理解】 

使用Lambda必须要有接口，并且要求接口中有且仅有一个抽象方法 必须有上下文环境，才能推导出Lambda对应的接口 根据局部变量的赋值得知Lambda对应的接口 Runnable r = () -> System.out.println("Lambda表达式"); 根据调用方法的参数得知Lambda对应的接口 public interface Addable { int add(int x, int y); } public interface Flyable { void fly(String s); } public class LambdaDemo { public static void main(String[] args) { // useAddable((int x,int y) -> { // return x + y; // }); //参数的类型可以省略 useAddable((x, y) -> { return x + y; }); // useFlyable((String s) -> { // System.out.println(s); // }); //如果参数有且仅有一个，那么小括号可以省略 // useFlyable(s -> { // System.out.println(s); // }); //如果代码块的语句只有一条，可以省略大括号和分号 useFlyable(s -> System.out.println(s)); //如果代码块的语句只有一条，可以省略大括号和分号，如果有return，return也要省略掉 useAddable((x, y) -> x + y); } private static void useFlyable(Flyable f) { f.fly("风和日丽，晴空万里"); } private static void useAddable(Addable a) { int sum = a.add(10, 20); System.out.println(sum); } } new Thread(() -> System.out.println("Lambda表达式")).start()

**Lambda表达式和匿名内部类的区别【理解】**

 所需类型不同 匿名内部类：可以是接口，也可以是抽象类，还可以是具体类 Lambda表达式：只能是接口 使用限制不同 如果接口中有且仅有一个抽象方法，可以使用Lambda表达式，也可以使用匿名内部类 如果接口中多于一个抽象方法，只能使用匿名内部类，而不能使用Lambda表达式 实现原理不同 匿名内部类：编译之后，产生一个单独的.class字节码文件 Lambda表达式：编译之后，没有一个单独的.class字节码文件。对应的字节码会在运行的时候动态生成

# Java占位符

[String类](https://so.csdn.net/so/search?q=String类&spm=1001.2101.3001.7020)的format()方法用于创建格式化的字符串以及连接多个字符串对象。熟悉C语言的同学应该记得C语言的sprintf()方法，两者有类似之处。format()方法有两种重载形式。

format(String format, Object... [args](https://so.csdn.net/so/search?q=args&spm=1001.2101.3001.7020)) 新字符串使用本地语言环境，制定字符串格式和参数生成格式化的新字符串。

format(Locale locale, String format, Object... args) 使用指定的语言环境，制定[字符串](https://so.csdn.net/so/search?q=字符串&spm=1001.2101.3001.7020)格式和参数生成格式化的字符串。

**1.不同数据类型到字符串**

1显示不同转换符实现不同数据类型到字符串的转换，如图所示。

| 转 换 符 | 说  明                                      | 示  例       |
| -------- | ------------------------------------------- | ------------ |
| %s       | 字符串类型                                  | "mingrisoft" |
| %c       | 字符类型                                    | 'm'          |
| %b       | 布尔类型                                    | true         |
| %d       | 整数类型（十进制）                          | 99           |
| %x       | 整数类型（十六进制）                        | FF           |
| %o       | 整数类型（八进制）                          | 77           |
| %f       | 浮点类型                                    | 99.99        |
| %a       | 十六进制浮点类型                            | FF.35AE      |
| %e       | 指数类型                                    | 9.38e+5      |
| %g       | 通用浮点类型（f和e类型中较短的）            |              |
| %h       | 散列码                                      |              |
| %%       | 百分比类型                                  | ％           |
| %n       | 换行符                                      |              |
| %tx      | 日期与时间类型（x代表不同的日期与时间转换符 |              |

测试用例

 ```
 public static void main(String[] args) {
         String str=null;
 
         str=String.format("Hi,%s", "王力");
 
         System.out.println(str);
 
         str=String.format("Hi,%s:%s.%s", "王南","王力","王张");          
 
         System.out.println(str);                         
 
         System.out.printf("字母a的大写是：%c %n", 'A');
 
         System.out.printf("3>7的结果是：%b %n", 3>7);
 
         System.out.printf("100的一半是：%d %n", 100/2);
 
         System.out.printf("100的16进制数是：%x %n", 100);
 
         System.out.printf("100的8进制数是：%o %n", 100);
 
         System.out.printf("50元的书打8.5折扣是：%f 元%n", 50*0.85);
 
         System.out.printf("上面价格的16进制数是：%a %n", 50*0.85);
 
         System.out.printf("上面价格的指数表示：%e %n", 50*0.85);
 
         System.out.printf("上面价格的指数和浮点数结果的长度较短的是：%g %n", 50*0.85);
 
         System.out.printf("上面的折扣是%d%% %n", 85);
 
         System.out.printf("字母A的散列码是：%h %n", 'A');
 
     }
 ```





 

 

 输出结果

 

Hi,王力 Hi,王南:王力.王张 字母a的大写是：A  3>7的结果是：false  100的一半是：50  100的16进制数是：64  100的8进制数是：144  50元的书打8.5折扣是：42.500000 元 上面价格的16进制数是：0x1.54p5  上面价格的指数表示：4.250000e+01  上面价格的指数和浮点数结果的长度较短的是：42.5000  上面的折扣是85%  字母A的散列码是：41 

 

**2搭配转换符的标志，如图所示。**

| 标  志 | 说  明                                                   | 示  例                  | 结  果           |
| ------ | -------------------------------------------------------- | ----------------------- | ---------------- |
| +      | 为正数或者负数添加符号                                   | ("%+d",15)              | +15              |
| −      | 左对齐                                                   | ("%-5d",15)             | \|15  \|         |
| 0      | 数字前面补0                                              | ("%04d", 99)            | 0099             |
| 空格   | 在整数之前添加指定数量的空格                             | ("% 4d", 99)            | \| 99\|          |
| ,      | 以“,”对数字分组                                          | ("%,f", 9999.99)        | 9,999.990000     |
| (      | 使用括号包含负数                                         | ("%(f", -99.99)         | (99.990000)      |
| #      | 如果是浮点数则包含小数点，如果是16进制或8进制则添加0x或0 | ("%#x", 99)("%#o", 99)  | 0x630143         |
| <      | 格式化前一个转换符所描述的参数                           | ("%f和%<3.2f", 99.45)   | 99.450000和99.45 |
| $      | 被格式化的参数索引                                       | ("%1$d,%2$s", 99,"abc") | 99,abc           |

测试用例

 

 public static void main(String[] args) {        String str=null;        //$使用        str=String.format("格式参数$的使用：%1$d,%2$s", 99,"abc");                   System.out.println(str);                             //+使用        System.out.printf("显示正负数的符号：%+d与%d%n", 99,-99);        //补O使用        System.out.printf("最牛的编号是：%03d%n", 7);        //空格使用        System.out.printf("Tab键的效果是：% 8d%n", 7);        //.使用        System.out.printf("整数分组的效果是：%,d%n", 9989997);        //空格和小数点后面个数        System.out.printf("一本书的价格是：% 50.5f元%n", 49.8);    }

 

 

输出结果

格式参数$的使用：99,abc 显示正负数的符号：+99与-99 最牛的编号是：007 Tab键的效果是：       7 整数分组的效果是：9,989,997 一本书的价格是：                                          49.80000元

**3.日期和事件字符串格式化**

在程序界面中经常需要显示时间和日期，但是其显示的 格式经常不尽人意，需要编写大量的代码经过各种算法才得到理想的日期与时间格式。字符串格式中还有%tx转换符没有详细介绍，它是专门用来格式化日期和时 间的。%tx转换符中的x代表另外的处理日期和时间格式的转换符，它们的组合能够将日期和时间格式化成多种格式。

常见日期和时间组合的格式，如图所示。

| 转 换 符 | 说  明                      | 示  例                           |
| -------- | --------------------------- | -------------------------------- |
| c        | 包括全部日期和时间信息      | 星期六 十月 27 14:21:20 CST 2007 |
| F        | “年-月-日”格式              | 2007-10-27                       |
| D        | “月/日/年”格式              | 10/27/07                         |
| r        | “HH:MM:SS PM”格式（12时制） | 02:25:51 下午                    |
| T        | “HH:MM:SS”格式（24时制）    | 14:28:16                         |
| R        | “HH:MM”格式（24时制）       | 14:28                            |

测试用例

public static void main(String[] args) {            Date date=new Date();                                            //c的使用            System.out.printf("全部日期和时间信息：%tc%n",date);                    //f的使用            System.out.printf("年-月-日格式：%tF%n",date);            //d的使用            System.out.printf("月/日/年格式：%tD%n",date);            //r的使用            System.out.printf("HH:MM:SS PM格式（12时制）：%tr%n",date);            //t的使用            System.out.printf("HH:MM:SS格式（24时制）：%tT%n",date);            //R的使用            System.out.printf("HH:MM格式（24时制）：%tR",date);        }

 

 

 

 

输出结果

全部日期和时间信息：星期一 九月 10 10:43:36 CST 2012 年-月-日格式：2012-09-10 月/日/年格式：09/10/12 HH:MM:SS PM格式（12时制）：10:43:36 上午 HH:MM:SS格式（24时制）：10:43:36 HH:MM格式（24时制）：10:43

 定义日期格式的转换符可以使日期通过指定的转换符生成新字符串。这些日期转换符如图所示。

 

 

 public static void main(String[] args) {            Date date=new Date();                                                //b的使用，月份简称            String str=String.format(Locale.US,"英文月份简称：%tb",date);                 System.out.println(str);                                                                                        System.out.printf("本地月份简称：%tb%n",date);            //B的使用，月份全称            str=String.format(Locale.US,"英文月份全称：%tB",date);            System.out.println(str);            System.out.printf("本地月份全称：%tB%n",date);            //a的使用，星期简称            str=String.format(Locale.US,"英文星期的简称：%ta",date);            System.out.println(str);            //A的使用，星期全称            System.out.printf("本地星期的简称：%tA%n",date);            //C的使用，年前两位            System.out.printf("年的前两位数字（不足两位前面补0）：%tC%n",date);            //y的使用，年后两位            System.out.printf("年的后两位数字（不足两位前面补0）：%ty%n",date);            //j的使用，一年的天数            System.out.printf("一年中的天数（即年的第几天）：%tj%n",date);            //m的使用，月份            System.out.printf("两位数字的月份（不足两位前面补0）：%tm%n",date);            //d的使用，日（二位，不够补零）            System.out.printf("两位数字的日（不足两位前面补0）：%td%n",date);            //e的使用，日（一位不补零）            System.out.printf("月份的日（前面不补0）：%te",date);        }

 

输出结果 

英文月份简称：Sep 本地月份简称：九月 英文月份全称：September 本地月份全称：九月 英文星期的简称：Mon 本地星期的简称：星期一 年的前两位数字（不足两位前面补0）：20 年的后两位数字（不足两位前面补0）：12 一年中的天数（即年的第几天）：254 两位数字的月份（不足两位前面补0）：09 两位数字的日（不足两位前面补0）：10 月份的日（前面不补0）：10

 

**4日期格式化** 

和日期格式转换符相比，时间格式的转换符要更多、更精确。它可以将时间格式化成时、分、秒甚至时毫秒等单位。格式化时间字符串的转换符如图所示。

| 转 换 符 | 说  明                                 | 示  例         |
| -------- | -------------------------------------- | -------------- |
| H        | 2位数字24时制的小时（不足2位前面补0）  | 15             |
| I        | 2位数字12时制的小时（不足2位前面补0）  | 03             |
| k        | 2位数字24时制的小时（前面不补0）       | 15             |
| l        | 2位数字12时制的小时（前面不补0）       | 3              |
| M        | 2位数字的分钟（不足2位前面补0）        | 03             |
| S        | 2位数字的秒（不足2位前面补0）          | 09             |
| L        | 3位数字的毫秒（不足3位前面补0）        | 015            |
| N        | 9位数字的毫秒数（不足9位前面补0）      | 562000000      |
| p        | 小写字母的上午或下午标记               | 中：下午英：pm |
| z        | 相对于GMT的RFC822时区的偏移量          | +0800          |
| Z        | 时区缩写字符串                         | CST            |
| s        | 1970-1-1 00:00:00 到现在所经过的秒数   | 1193468128     |
| q        | 1970-1-1 00:00:00 到现在所经过的毫秒数 | 1193468128984  |

测试代码

 

 

 

public static void main(String[] args) {        Date date = new Date();        //H的使用        System.out.printf("2位数字24时制的小时（不足2位前面补0）:%tH%n", date);        //I的使用        System.out.printf("2位数字12时制的小时（不足2位前面补0）:%tI%n", date);        //k的使用        System.out.printf("2位数字24时制的小时（前面不补0）:%tk%n", date);        //l的使用        System.out.printf("2位数字12时制的小时（前面不补0）:%tl%n", date);        //M的使用        System.out.printf("2位数字的分钟（不足2位前面补0）:%tM%n", date);        //S的使用        System.out.printf("2位数字的秒（不足2位前面补0）:%tS%n", date);        //L的使用        System.out.printf("3位数字的毫秒（不足3位前面补0）:%tL%n", date);        //N的使用        System.out.printf("9位数字的毫秒数（不足9位前面补0）:%tN%n", date);        //p的使用        String str = String.format(Locale.US, "小写字母的上午或下午标记(英)：%tp", date);        System.out.println(str);         System.out.printf("小写字母的上午或下午标记（中）：%tp%n", date);        //z的使用        System.out.printf("相对于GMT的RFC822时区的偏移量:%tz%n", date);        //Z的使用        System.out.printf("时区缩写字符串:%tZ%n", date);        //s的使用        System.out.printf("1970-1-1 00:00:00 到现在所经过的秒数：%ts%n", date);        //Q的使用        System.out.printf("1970-1-1 00:00:00 到现在所经过的毫秒数：%tQ%n", date);    }

 

 

 

输出结果

 

 

2位数字24时制的小时（不足2位前面补0）:11 2位数字12时制的小时（不足2位前面补0）:11 2位数字24时制的小时（前面不补0）:11 2位数字12时制的小时（前面不补0）:11 2位数字的分钟（不足2位前面补0）:03 2位数字的秒（不足2位前面补0）:52 3位数字的毫秒（不足3位前面补0）:773 9位数字的毫秒数（不足9位前面补0）:773000000 小写字母的上午或下午标记(英)：am 小写字母的上午或下午标记（中）：上午 相对于GMT的RFC822时区的偏移量:+0800 时区缩写字符串:CST 1970-1-1 00:00:00 到现在所经过的秒数：1347246232 1970-1-1 00:00:00 到现在所经过的毫秒数：1347246232773

 

 