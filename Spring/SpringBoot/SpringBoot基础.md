# 1基础介绍

**SpringBoot工程运行后初始化Spring容器，扫描引导类所在包加载bean**

## **1.1  starter**

        SpringBoot中常见项目名称，定义了当前项目使用的所有依赖坐标，以达到**减少依赖配置**的目的

## **1.2 parent**

        所有SpringBoot项目要继承的项目，定义了若干个坐标版本号（依赖管理，而非依赖），以达到减少依赖冲突的目的

        spring-boot-starter-parent各版本间存在着诸多坐标版本不同

## **1.3 实际开发**

        使用任意坐标时，仅书写GAV中的G和A，V由SpringBoot提供，除非SpringBoot未提供对应版本V

## **1.4.如发生坐标错误，再指定Version（要小心版本冲突）**

**引导类**

```
@SpringBootApplication
public class Springboot01QuickstartApplication {
	public static void main(String[] args) { 
		SpringApplication.run(Springboot01QuickstartApplication.class, args); 
	}
}
```

 SpringBoot的引导类是Boot工程的执行入口，运行main方法就可以启动项目

    SpringBoot工程运行后初始化Spring容器，扫描引导类所在包加载bean

# **2REST开发**

## **2.1.REST简介**

**REST** (Representational State Transfer)，表现形式状态转换

**传统风格资源描述形式**

```
http: / / localhost/user/getById?id=1

http: / / localhost/user/saveUser
```

**REST风格描述形式**

http: / /localhost/user/1

**优点:**

隐藏资源的访问行为，无法通过地址得知对资源是何种操作

书写简化

按照REST风格访问资源时使用行为动作区分对资源进行了何种操作

```
- http:/ / localhost/users 查询全部用户信息 **GET查询**
- http:/ / localhost/users/1 查询指定用户信息GET(查询)
- http: / / localhost/users 添加用户信息 **POST(新增/保存)**
- http:/ / localhost/users 修改用户信息 **PUT（修改/更新)**
- http: / / localhost/users/1 删除用户信息 **DELETE (删除)**
```

上述行为是约定方式，约定不是规范，可以打破，所以称REST风格，而不是REST规范

描述**模块的名称通常使用复数**，也就是加s的格式描述，表示此类资源，而非单个资源，例如: users、books、account...

**根据REST风格对资源进行访问称为RESTful**

**可以直接对注解按ctrl+b查看结构**

**@Pathvariable**  **//请求参数来自路径注解**

**@RequstMapping(value ="\**\*/{****参数****}")**

```
@RequestMapping(value = "/users/{id}" ,method = RequestMethod.DELETE)
@ResponseBody 
public string delete(@Pathvariable Integer id){ 
	System.out.printin("user delete. . ." + id); 
	return "{'module":" user delete'}";
}
```



## 2.2@RequestBody@RequestParam@PathVariable

**区别**

- @RequestParam用于接收url地址传参或表单传参
- **@RequestBody用于接收json数据**
- @PathVariable用于接收路径参数，使用{**参数名称**}描述路径参数

**应用**

- 后期开发中，发送请求参数超过1个时，以json格式为主@RequestBody应用较广
- 如果发送非json格式数据，选@Requestparam接收请求参数
- 采用RESTful进行开发，当参数数量较少时，例如1个，可以采用@PathVariable接收请求路径变量，通常用于传递id值

## **2.3.restful快速开发**

**@RestController=****@Controller+@ResponseBody**

**@PostMapping=@RequstMapping(value="",method=RequstMethod.POST)添加**

**@PutMapping=@RequstMapping(value="",method=RequstMethod.PUT)修改**

**@GetMapping=@RequstMapping(value="",method=RequstMethod.GET)查询**

**@DeleteMapping=@RequstMapping(value="",method=RequstMethod.DELETE)删除**

删除整行------- 在不选中的情况下 **Ctrl+X** 

这本来是剪切当前，只要不选中代码按**Ctrl+X**算是剪切当前一整行，

还有一个真正是删除当前一整行的是 Ctrl+Y，这个Y键比较远，哪怕小编手大指长也甘拜下风，所以我还是感觉**Ctrl+X**好使。

```
@RestController @RequestMapping("books") 
public class BookController {
/*@RequestMapping(value = "/book1/{param}",method = RequestMethod.GET )*/    @GetMapping("book1/{param}")   
	public String test1(@PathVariable String param){  
    System.out.println(param);        
    System.out.println("springboot is running");   
    return"springboot is running";   
    }
```

# **3、基础配置**

写哪？写什么？格式有什么要求？

基础+使用一块讲解

**属性配置**

## 3.1、直接搞application.properties

https://docs.spring.io/spring-boot/docs/current/reference/html/application-properties.html#application-properties

在这里查找

例如serve.port=8080 #修改端口号

## **3.2.application.yml(主流)**

(亚麦澳)

（&读z'da）

server:  port: 80

3.application.yaml

server:  port:82

## 3.3加载优先级

```
properties>yml>yaml
```

不同配置文件中相同配置按照加载优先级相互覆盖，**不同配置文件中不同配置全部保留**	

如果不显示

**project structure-facets-spring-绿叶设置中添加**

**格式**

yaml

### 3.3.1YAML (YAML Ain 't Markup Language) ，一种数据序列化格式优点:

- 容易阅读
- 容易与脚本语言交互
- 以数据为核心，重数据轻格式

**YAML文件扩展名**

**.yml（主流).yaml**

3.3.1.1yaml语法规则

- 大小写敏感
- 属性层级关系使用多行描述，每行结尾使用冒号结束
- 使用缩进表示层级关系，同层级左侧对齐，只允许使用空格（不允许使用Tab键)
- **属性值前面添加空格**（属性名与属性值之间使用冒号+空格作为分隔)
- **#表示注释**

```
server:
  port: 80
country: china
  province:shandong
    city:zaozhuang
      veliiger:tangyin
#多个数据 -后边还是要空格
users:
    - name: 蔡徐坤
      age: 99
    - name: 肖干
      age: 55   
```



**核心规则：数据前面要加空格与冒号隔开**

boolean: TRUE 	#TRUE,true,True,FALSE,false，False均可

float: 3.14 		#6.8523015e+5  #支持科学计数法

int: 123	                 #0b1010_0111_0100_1010_1110    **#支持二进制、八进制、十六进制**

null: ~			#使用~表示null

string: HelloWorld    #字符串可以直接书写

string2: "Hello World"  #可以使用双引号包裹特殊字符

date: 2018-02-17   #日期必须使用yyyy-MM-dd格式

datetime: 2018-02-17T15:02:31+08:00  #时间和日期之间使用T连接，最后使用+代表时区

users:            #对象数组格式

\- name: Tom  age: 4

\- name: Jerry  age: 5 users: #对象数组格式二

 \-  

​    name: Tom    age: 4

 \-  name: Jerry

​     age: 5

\#对象数组缩略格式

users2: [ { name:Tom , age:4 } , { name:Jerry , age:5 } ]

server:			@Value("${server.port})

  port:  82   --------->String s;

- 在配置文件中可以使用属性名引用方式引用属性

baseDir: /usr/local/fire

center:

dataDir: ${baseDir}/data

tmpDir: ${baseDir}/tmp

logDir: ${baseDir}/log

msgDir: ${baseDir}/msgDir

- 属性值中如果出现转移字符，需要使用双引号包裹

lesson: "Spring\tboot\nlesson"

- 封装全部数据到Environment对象

@Autowired private Environment env; System.out.println(env.getProperty("变量名称"));

- 自定义对象封装指定数据

enterprise:

name: itcast

age: 16

tel: 4006184000

subject:

   \- Java

   \- 前端

   \- 大数据

@Component @ConfigurationProperties(prefix = "enterprise") public class Enterprise { private String name; private Integer age; private String[] subject;  } @RestController @RequestMapping("/books")  public class BookController { @Autowired private Enterprise enterprise;  }

**1．使用@ConfigurationProperties注解绑定配置信息到封装类中**

**2．封装类需要定义为Spring管理的bean，否则无法进行属性注入**

## 3.4、路径配置

```
server:
  port: 8080
  servlet:
    context-path: /test
```



# 4整合第三方技术

## 4.1.整合Junit

 名称：**@SpringBootTest**

 类型：测试类注解

  位置：测试类定义上方

 作用：设置JUnit加载的SpringBoot启动类

    范例：

默认路径：

```
@SpringBootTest
class Springboot05JUnitApplicationTests {}
```

修改了测试类的路径

`@SpringBootTest(classes = Springboot05JUnitApplication.class)`

`class Springboot07JUnitApplicationTests {}`

**如果测试类在SpringBoot启动类的包或子包中，可以省略启动类的设置，也就是省略classes的设定**

## 4.2.整合Mybatis

**犯的错误**

### 4.2.1.url中少了jdbc:mysql:

2.mapper中select....where id=#{少了参数}

**总结**

1．勾选MyBatis技术，也就是导入MyBatis对应的starter

2．数据库连接相关信息转换成配置

3．数据库SQL映射需要添加@Mapper被容器识别到

企业中的springboot版本会低一些（举例：2.4.1）

The server time zone value "i白◆Z"白' is unrecognized

**解决**

SpringBoot版本低于2.4.3(不含)，Mysql驱动版本大于8.0时，需要在url连接串中配置时区

或在MySQL数据库端配置时区解决此问题

`jdbc:mysql://localhost:3306/ssm_db?serverTimezone=UTC`

然后捏 以后就用

`com.mysql.cj.jdbc` 这个diver

**总结：**

1.MySQL 8.X驱动强制要求设置时区

修改url，添加serverTimezone设定

修改MySQL数据库配置（略)

2．驱动类过时，提醒更换为com.mysql.cj.jdbc.Driver

## 4.3.整合MyBatis-Plus

- MyBatis-Plus与MyBatis区别

导入坐标不同

数据层实现简化

**致命错误：数据库中表的列明不能为desc，傻屌mp识别不出来。**

①：手动添加SpringBoot整合MyBatis-Plus的坐标，可以通过mvnrepository获取

```
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.5.1</version>
</dependency>
```

**由于SpringBoot中未收录MyBatis-Plus的坐标版本，需要指定对应的Version**

②：定义数据层接口与映射配置，继承BaseMappep

在BookDao上加@Mapper或者是在引导类上加@MapperScan("com.itheima.dao")

public interface BookDao extends BaseMapper<Book> { }

## 4.4.整合Druid

**数据源配置：**

①

```
spring:
	datasource:
		driver-class-name: com.mysql.cj.jdbc.Driver
			url: jdbc:mysql://localhost:3306/ssm_db?serverTimezone=UTC
			username: root
			password: root
			type: com.alibaba.druid.pool.DruidDataSource
```

②

```
spring:
 datasource:
  druid:
      driver-class-name: com.mysql.cj.jdbc.Driver
      url: jdbc:mysql://localhost:3306/ssm_db?serverTimezone=UTC
      username: root
      password: root
```

<u>我把driver-class-name写成了driver报错！！！！</u>

```
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid-spring-boot-starter</artifactId>
    <version>1.2.8</version>
</dependency>
```

## 4.5整合第三方技术通用方式：

**导入对应的starter**

根据提供的配置格式，配置非默认值对应的配置项

# 5SSMP实例

## 5.1.简化pojo

**技术：**

**lombok**

```
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
</dependency>
```

@Getter为类安排get方法

@Setter为类安排set方法

**@Data=@****Getter+@Setter**

@AllArgsConstructor 全参构造

@NoArgsConstructor 无参构造

## 5.2.数据层开发

①mybatisplus

②druid

```
@Mapper
public interface BookDao extends BaseMapper<Book> {
    //如果不用mp的技术的话
    @Select("select * from book where id=#{id}")
    Book getbyid(Integer id);
}
```

```
mybatis-plus:
 configuration:
    global-config:
        db-config:
            log-prefix: 此处设置数据库表前缀
            id-type:auto 自增
      configuration:
              //打印日志
             log-impl: org.apache.ibatis.logging.stdout.StdoutImpl
```



启动分页

```
@Configuration
public class MPConfig {
    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor(){
        MybatisPlusInterceptor mybatisPlusInterceptor=new MybatisPlusInterceptor();
        mybatisPlusInterceptor.addInnerInterceptor(new PaginationInnerInterceptor());
        return mybatisPlusInterceptor;
    }
}
```



**1. 使用QueryWrapper对象封装查询条件**

```
@Test
void testGetByCondition(){
QueryWrapper qw = new QueryWrapper();
qw.like("name","Spring");
bookDao.selectList(qw);
}
```



**2. 推荐使用LambdaQueryWrapper对象**

```
@Test
void testGetByCondition(){
QueryWrapper qw = new QueryWrapper();
qw.like("name","Spring");
bookDao.selectList(qw);
}
```



3. 所有查询操作封装成方法调用

4. 查询条件支持动态条件拼装

```
@Test
void testGetByCondition(){
String name = "Spring";
IPage page = new Page(1,10);
LambdaQueryWrapper lqw = new LambdaQueryWrapper();
lqw.like(Strings.isNotEmpty(name),Book::getName,"Spring");
bookDao.selectPage(page,lqw);
}
```



## 5.3.业务层开发

以前的那种

BookService---》impl.BookServiceImpl将不存在啦

**采用MP的解决方案**

**1.接口**

```
public interface IBookService extends IService<Book> {
自己写的不能和原来的一样，@Overwrite检测
}
```

**2.实现类**

```
@Service
public class IBookServiceImpl extends ServiceImpl<BookDao, Book> implements IBookService {
}
```

**3.测试**

```
@SpringBootTest
public class IBookServiceTest {
    @Autowired
    IBookService bookService=new IBookServiceImpl();
    @Test
    public void test1(){

        System.out.println( bookService.save(new Book(null,"负能量","王一波自传","太恶心了")));
    }@Test
    public void test2(){
       System.out.println(bookService.update(new Book(4,"正能量","喜洋洋","啊咩咩"),null));
    }@Test
    public void test3(){
        System.out.println(bookService.removeById(2));
    }@Test
    public void test4(){
        System.out.println(bookService.list());

    }@Test
    public void test5(){
        Page<Book> page=new Page<>(1,2);
        Page<Book> page1 = bookService.page(page);
        System.out.println(page1.getRecords());

    }@Test
    public void test6(){
        bookService.getById(3);

    }
}
```

```
bookService.update(new Book(4,"正能量","喜洋洋","啊咩咩"),null)
==>  Preparing: UPDATE book SET type=?, name=?, des=?
==> Parameters: 正能量(String), 喜洋洋(String), 啊咩咩(String)
<==    Updates: 4
```

:warning:导致所有的数据都成了喜洋洋，是因为没有设置条件，他的执行语句就是UPDATE book SET type=?, name=?, des=?！没有where!!

## 5.4.表现层开发

5.4.1.基于Restful进行表现层接口开发

```
@RestController
@RequestMapping("books")
public class BookController {
    @Autowired
    private IBookService iBookService;

    @GetMapping
    public List<Book> getlist(){
        return iBookService.list();
    }
    @PostMapping
    public void save(@RequestBody  Book book){
        iBookService.save(book);
    }
    @PutMapping
    public void update(@RequestBody Book book){
        iBookService.updateById(book);
    }
    @DeleteMapping("{id}")
    public void delete(@PathVariable Integer id){
        iBookService.removeById(id);
    }
    @GetMapping("{id}")
    public Book getbyid(@PathVariable  Integer id){
        Book byId = iBookService.getById(id);
        return byId;
    }
    @GetMapping("{curentpage}/{pagesize}")
    public Page<Book> page(@PathVariable  int curentpage,@PathVariable int pagesize){
        Page<Book> page=new Page<>(curentpage,pagesize);
        return iBookService.page(page,null);
    }
```

其中

**实体数据：@RequestBody 路径变量：@PathVariable**

**分页：**@GetMapping("{curentpage}/{pagesize}")

5.4.2.前后端联调*

设计表现层返回结果的模型类，用于后端与前端进行数据格式统一，也称为**前后端数据协议**

{                         {

"flag" : true,		"flag" : true,

查询数据不存在	查询过程抛异常

" data" : null    		"data" : null

}				}

```
@Data

public class R {
    private Boolean flag;
    private Object object;

    public R(Boolean aBoolean) {
        this.flag = aBoolean;
    }
    public R(Boolean aBoolean, Object object) {
        this.flag = aBoolean;
        this.object = object;
    }
}
```



5.4.3.前端

------

**重要内容**

多人同时操作一个数据库，可能你删完了，然后别人继续编辑，就会导致他无法编辑，因为数据已经给你删除了，说以要充分利用flag进行判断。

如果说flag=true才弹出编辑窗口。

------

前后端分离结构设计中页面归属前端服务器

 单体工程中页面放置在resources目录下的static目录中（出现错误**建议执行clean**）

1．单体项目中页面放置在resources/static目录下
2. created钩子函数用于初始化页面时发起调用
3．页面使用axios发送异步请求获取数据后确认前后端是否联通
①获取数据
\1. 将查询数据返回到页面，利用前端数据双向绑定进行数据展示
//列表getAll() {axios.get("/books").then(**(res)=>{this.dataList = res.data.data;})**;},
其中
初始化:create():getall()
网页创建时就调用
**②添加数据**
**//添加handleAdd () {//发送异步请求axios.post("/books",this.formData).then((res)=>{//如果操作成功，关闭弹层，显示数据**

**if(res.data.flag){**

**this.dialogFormVisible = false;**

**this.$message.success("添加成功");}**

**else {this.$message.error("添加失败");}**

**}).finally(()=>{this.getAll();});}**

**如果getall不在axios方法体系里面，就不会刷新页面**

//弹出添加窗口

handleCreate() {this.dialogFormVisible = true;},

//重置表单

resetForm() {this.formData = {};},

//弹出添加窗口

handleCreate() {this.dialogFormVisible = true;this.resetForm();},

//取消

cancel(){this.dialogFormVisible = false;this.$message.info("操作取消");},

③删除数据

1．请求方式使用Delete调用后台对应操作

axios.delete("/books/"+row.id)

\2. 添加操作结束后动态刷新页面加载数据

finally(()=>{this.getall()})

\3.  删除操作前弹出提示框避免误操作

this.$confirm("窗口内容","窗口名",{type:"类型"}).then(()=>{成功}).catch()=>{失败})

\4. 根据操作结果不同，显示对应的提示信息

flag判断

handleDelete(row) {    this.$confirm("会删除哦","提示",{type:"info"}).then(()=>{           axios.delete("/books/"+row.id).then((res)=>{        if (res.data.flag) {            this.$message.success("删除成功")}        else {            this.$message.error("删除失败")}    }).finally(()=>    {this.getAll()})    }).catch(()=>{        this.$message.info("取消")    }) },

**？？？三目运算我也能忘？**

**(关系表达式  结果是布尔值) ？ 表达式1 ： 表达式2;**

④异常处理

//返回Responsebody @RestControllerAdvice public class ProjectExceptionAdvice {    //拦截所有异常    @ExceptionHandler    // @ExceptionHandler(**.class)可以单独设置异常处理    public R doException(Exception e){        //记录日志 //发送消息给运维 //发送邮件给开发人员,ex对象发送给开发人员        return new R("服务器故障了");    } }

太骚了，这个操作

public R( String msg) {    this.flag = false;    this.msg = msg; }

修改表现层返回结果的模型类，封装出现异常后对应的信息 flag：false Data: null 消息(msg): 要显示信息

@Data

public class R{

private Boolean flag;

private Object data;

private String msg;

public R(Boolean flag,Object data,String msg){

this.flag = flag

;this.data = data;

this.msg = msg;}

}

**业务消息一致性处理**

- 业务消息一致性处理

@PostMapping

public R save(@RequestBody Book book) throws IOException {

Boolean flag = bookService.insert(book);

return new R(flag , flag ? "添加成功^_^" : "添加失败-_-!");}

- **目的：国际化**

//添加handleAdd () {//发送ajax请求axios.post("/books",this.formData).then((res)=>{if(res.data.flag){this.dialogFormVisible = false;this.$message.success(res.data.msg);}

else {this.$message.error(res.data.msg);}}).finally(()=>{this.getAll();});},

页面消息处

遇到的问题

if( **currentPage** > page.getPages())

{page = bookService.getPage((int)page.getPages(), pageSize);}