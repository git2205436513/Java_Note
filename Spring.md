# Spring
```
2020/3/1 fanfuni
在此感谢狂神说java老师
https://space.bilibili.com/95256449?from=search&seid=2289407580483923837
```
## 简介
- Spring : 春天 --->给软件行业带来了春天
- 2002年，Rod Jahnson首次推出了Spring框架雏形interface21框架。
- 2004年3月24日，Spring框架以interface21框架为基础，经过重新设计，发布了1.0正式版。
- Spring理念 : 使现有技术更加实用 . 本身就是一个大杂烩 , 整合现有的框架技术

## 优点
- Spring是一个开源免费的框架,容器。
- Spring是一个轻量级的框架,非侵入式的。
```
侵入式（引入或继承了别的包或框架）
从字面意思理解，就是你的代码里已经嵌入了别的代码，这些代码可能是你引入过的框架，也可能是你通过接口继承得来的（比如：java中的继承），这样你就可以拥有侵入代码的一些功能。所以我们就称这段代码是侵入式代码。非要说侵入式代码的优点：通过侵入代码与你的代码结合可以更好的利用侵入代码提供给的功能。缺点：框架外代码就不能使用了，不利于代码复用。依赖太多重构代码太痛苦了。
非侵入式（没有依赖，自主研发）
正好与侵入式相反，你的代码没有引入别的包或框架，完完全全是自主开发。比如golang中的结构体中的字段组合，这是非侵入式的，我完完全全可以其中某个字段的方法集合，或者我可以通过实现自己的方法集合从而达到剥离依赖关系的目的。优点：代码可复用，方便移植。非侵入式也体现了代码的设计原则：高内聚，低耦合
```
- 控制反转 IoC,面向切面 Aop
- 对事物的支持,对框架的支持

一句话概括：Spring是一个轻量级的控制反转(IoC)和面向切面(AOP)的容器（框架）
---
## Spring组成
- **核心容器**：核心容器提供 Spring 框架的基本功能。核心容器的主要组件是 BeanFactory，它是工厂模式的实现。BeanFactory 使用控制反转（IOC） 模式将应用程序的配置和依赖性规范与实际的应用程序代码分开。
- **Spring** 上下文：Spring 上下文是一个配置文件，向 Spring 框架提供上下文信息。Spring 上下文包括企业服务，例如 JNDI、EJB、电子邮件、国际化、校验和调度功能。
- **Spring AOP**：通过配置管理特性，Spring AOP 模块直接将面向切面的编程功能 , 集成到了 Spring 框架中。所以，可以很容易地使 Spring 框架管理任何支持 AOP的对象。Spring AOP 模块为基于 Spring 的应用程序中的对象提供了事务管理服务。通过使用 Spring AOP，不用依赖组件，就可以将声明性事务管理集成到应用程序中。
- **Spring DAO**：JDBC DAO 抽象层提供了有意义的异常层次结构，可用该结构来管理异常处理和不同数据库供应商抛出的错误消息。异常层次结构简化了错误处理，并且极大地降低了需要编写的异常代码数量（例如打开和关闭连接）。Spring DAO 的面向 JDBC 的异常遵从通用的 DAO 异常层次结构。
- **Spring ORM**：Spring 框架插入了若干个 ORM 框架，从而提供了 ORM 的对象关系工具，其中包括 JDO、Hibernate 和 iBatis SQL Map。所有这些都遵从 Spring 的通用事务和 DAO 异常层次结构。
- **Spring Web模块**：Web 上下文模块建立在应用程序上下文模块之上，为基于 Web 的应用程序提供了上下文。所以，Spring 框架支持与 Jakarta Struts 的集成。Web 模块还简化了处理多部分请求以及将请求参数绑定到域对象的工作。
- **Spring MVC框架**：MVC 框架是一个全功能的构建 Web 应用程序的 MVC 实现。通过策略接口，MVC 框架变成为高度可配置的，MVC 容纳了大量视图技术，其中包括 JSP、Velocity、Tiles、iText 和 POI。

---
## IOC
### 是什么？
控制反转IoC(Inversion of Control)，是一种设计思想，DI(依赖注入)是实现IoC的一种方法
### 为啥要有？怎么来的？
沿寻最开始J2EE的写法，我们要书写Dao接口、DAO实现、Service接口、Service实现。
Dao接口
```
public interface UserDao {
    public void getUser();
}
```
Dao实现
```
public class UserDaoImpl implements UserDao {
    @Override
    public void getUser() {
        System.out.println("获取用户数据");
    }
}
```
Service接口
```
public interface UserService {
    public void getUser();
}
```
Service实现
```
public class UserServiceImpl implements UserService {
    private UserDao userDao = new UserDaoImpl();

    @Override
    public void getUser() {
        userDao.getUser();
    }
}
```
似乎一切都没有问题，但是现在提出需求，Dao实现需要新增一个
```
public class UserDaoMySqlImpl implements UserDao {
    @Override
    public void getUser() {
        System.out.println("MySql获取用户数据");
    }
}
```
那么我们如果需要调用这个实现，则需要在Service实现中new一个对象指向UserDaoMySqlImpl，那么如果后续还有实现呢？如果每一次都要修改Service实现，不是太麻烦了吗？

那么我们该如何去解决这种高耦合的情况呢？

我们可以封装Service实现，利用set来实现
```
public class UserServiceImpl implements UserService {
    private UserDao userDao;
    // 利用set实现
    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }

    @Override
    public void getUser() {
        userDao.getUser();
    }
}
```
这样子调用就变得简单了起来
```
@Test
public void test(){
    UserServiceImpl service = new UserServiceImpl();
    service.setUserDao( new UserDaoImpl() );
    service.getUser();
    //那我们现在又想用SQL去实现呢
    service.setUserDao( new UserDaoMysqlImpl() );
    service.getUser();
}
```
这样子似乎没发生太大的变化，但是从思想上就已经开始转变了。我们解放了双手，不用再去修改Service实现，不再主动去创建对象，将这个东西交给了调用者。这就是IOC的原型。
### IOC本质
所谓控制反转就是：获得依赖对象的方式反转了。以前由我们自己创建对象，而现在交给程序来创建.

IoC是Spring框架的核心内容，使用多种方式完美的实现了IoC，可以使用XML配置，也可以使用注解，新版本的Spring也可以零配置实现IoC。

控制反转是一种通过描述（XML或注解）并通过第三方去生产或获取特定对象的方式。在Spring中实现控制反转的是IoC容器，其实现方法是依赖注入（Dependency Injection,DI）。

### 使用Spring IOC
- 创建Maven工程导入jar包
```
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.2.0.RELEASE</version>
</dependency>
为啥不导入core包二是web-mvc？ 因为web-mvc依赖了Spring的很多包，顺带帮我们导入了以后也懒得麻烦
```
1.创建一个实体类
```
public class Hello {
    private String name;

    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }

    public void show(){
        System.out.println("Hello,"+ name );
    }
}
```
2.编写Spring文件 
```
https://docs.spring.io/spring/docs/5.2.3.RELEASE/spring-framework-reference/core.html#spring-core
1.2.1
```
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--bean就是java对象 , 由Spring创建和管理-->
<!--
    设置别名：在获取Bean的时候可以使用别名获取  alias
    id 是bean的标识符,要唯一,如果没有配置id,name就是默认标识符
    如果配置id,又配置了name,那么name是别名
    name可以设置多个别名,可以用逗号,分号,空格隔开
    如果不配置id和name,可以根据applicationContext.getBean(.class)获取对象;

    class是bean的全限定名=包名+类名
    
-->
    <alias name="1" alias="good"/>
    <bean id="hello" name="1,2,3,4"  class="com.funi.pojo.Hello">
        <property name="name" value="Spring"/>
        
    </bean>

</beans>
```
```
当然还有一种方法是通过有参函数来创建,这样beans.xml就由三种编写方式type/index/name
public class Hello {
    private String name;

    public Hello(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public void show(){
        System.out.println("Hello,"+ name );
    }
}

   <alias name="1" alias="good"/>
    <bean id="hello" name="1,2,3,4"  class="com.funi.pojo.Hello">
<!--        <constructor-arg name="name" value="hello"/>-->
<!--        <constructor-arg index="0" value="hello"/>-->
        <constructor-arg type="java.lang.String" value="hello"/>
    </bean>
```
3.编写测试类
```
ApplicationContext context = new ClassPathXmlApplicationContext("services.xml", "daos.xml");
源自官网1.2.2
```
```
  @Test
    public void test1(){
        //读取beans.xml 获取beans中注册的对象
        ApplicationContext Context = new ClassPathXmlApplicationContext("beans.xml");
        //getBean 获取对象中的具体值
        Hello hello = (Hello) Context.getBean("hello");
        hello.show();
    }
```
- Hello对象由谁创造? 由Spring创建
- Hello对象的内置方法/属性由谁设置? 由Spring容器设置

这个过程就叫做控制反转:
- 控制:谁来控制对象的创建,以前由程序来控制,现在由spring来控制
- 反转:程序不再创建对象,反而接收对象.

(DI)依赖注入:利用set来进行注入

如果我们删除Hello中的set方法会发生什么?
```
invalid property 'name' of bean class [com.funi.pojo.Hello]: Bean property 'name' is not writable or has an invalid setter method.
```
对象中必须要有set方法才能实现DI

IOC是一种编程思想，由主动的编程变成被动的接收.所谓的IOC,一句话搞定:对象由Spring 来创建,管理,装配 !

## DI 依赖注入
- 依赖:指Bean对象的创建依赖于容器
- 注入:指Bean对象所依赖的资源,由容器来设置和装配

### 构造器注入 
### set注入
要求被注入的属性,必须由set方法。如果数据类型为Boolean，则为is方法

测试类编写
```
public class Address {

    private String address;

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }
}

package com.funi.pojo;

import java.util.List;
import java.util.Map;
import java.util.Properties;
import java.util.Set;

public class Student {

    private String name;
    private Address address;
    private String[] books;
    private List<String> hobbys;
    private Map<String,String> card;
    private Set<String> games;
    private String wife;
    private Properties info;

    public void setName(String name) {
        this.name = name;
    }

    public void setAddress(Address address) {
        this.address = address;
    }

    public void setBooks(String[] books) {
        this.books = books;
    }

    public void setHobbys(List<String> hobbys) {
        this.hobbys = hobbys;
    }

    public void setCard(Map<String, String> card) {
        this.card = card;
    }

    public void setGames(Set<String> games) {
        this.games = games;
    }

    public void setWife(String wife) {
        this.wife = wife;
    }

    public void setInfo(Properties info) {
        this.info = info;
    }

    public void show(){
        System.out.println("name="+ name
                + ",address="+ address.getAddress()
                + ",books="
        );
        for (String book:books){
            System.out.print("<<"+book+">>\t");
        }
        System.out.println("\n爱好:"+hobbys);

        System.out.println("card:"+card);

        System.out.println("games:"+games);

        System.out.println("wife:"+wife);

        System.out.println("info:"+info);

    }
}
```
- 常量注入
```
<bean id="student" class="com.kuang.pojo.Student">
    <property name="name" value="小明"/>
</bean>
```
- Bean注入(注意这里的引用ref)
```
<bean id="addr" class="com.kuang.pojo.Address">
    <property name="address" value="重庆"/>
</bean>

<bean id="student" class="com.kuang.pojo.Student">
    <property name="name" value="小明"/>
    <property name="address" ref="addr"/>
</bean>
```
- 数组注入
```
<bean id="student" class="com.kuang.pojo.Student">
    <property name="name" value="小明"/>
    <property name="address" ref="addr"/>
    <property name="books">
        <array>
            <value>西游记</value>
            <value>红楼梦</value>
            <value>水浒传</value>
        </array>
    </property>
</bean>
```
- List注入
```
<property name="hobbys">
    <list>
        <value>听歌</value>
        <value>看电影</value>
        <value>爬山</value>
    </list>
</property>
```
- Map注入
```
<property name="card">
    <map>
        <entry key="中国邮政" value="456456456465456"/>
        <entry key="建设" value="1456682255511"/>
    </map>
</property>
```
- set注入
```
<property name="games">
    <set>
        <value>LOL</value>
        <value>BOB</value>
        <value>COC</value>
    </set>
</property>
```
- NUll注入
```
<property name="wife"><null/></property>
```
- properties注入
```
<property name="info">
    <props>
        <prop key="学号">20190604</prop>
        <prop key="性别">男</prop>
        <prop key="姓名">小明</prop>
    </props>
</property>
```

---


## Bean的作用域 (在bean属性中的scope中配置)
在Spring中，bean是组成应用程序的主体及由Spring IOC容器所管理的对象。简单地讲，bean就是由IoC容器初始化、装配及管理的对象。
- **singleton**：在Spring容器中仅存在一个Bean实例，Bean以单例方式存在，默认值。创建容器时就存在。
- **prototype**：每次从容器中调用Bean时，都返回一个新的实例，即每次调用getBean（）时，相当于执行new XXXBean（）。在获取bean时才会被创建。
一般来说对有状态的bean使用prototype反之singleton
- **session**：同一个HTTP Session共享一个Bean，不同Session使用不同Bean，仅适用于WebApplicationCOntext环境。当HTTP Session最终被废弃的时候，在该HTTP Session作用域内的bean也会被废弃掉。
- **request**：每次HTTP请求都会创建一个新的Bean，仅适用于WebApplicationCOntext环境。当处理请求结束，request作用域的bean实例将被销毁。


---
## 自动装配
Spring中bean有三种装配机制，分别是：
- 在xml中显式配置
- 在java中显式配置
- **隐式的bean发现机制和自动装配**

### Spring自动装配的两个操作
- 组件扫描：spring会自动发现应用上下文中所创建的bean；
- 自动装配：spring自动满足bean之间的依赖

创建测试用例
```
public class Cat {
    public void shout() {
        System.out.println("miao~");
    }
}
public class Dog {
    public void shout() {
        System.out.println("wang~");
    }
}

public class People {
    private Cat cat;
    private Dog dog;
    private String name;
}

<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="dog" class="com.funi.pojo.Dog"/>
    <bean id="cat" class="com.funi.pojo.Cat"/>

    <bean id="people" class="com.funi.pojo.People">
        <property name="cat" ref="cat"/>
        <property name="dog" ref="dog"/>
        <property name="name" value="qinjiang"/>
    </bean>
</beans>

public class mytest {
    @Test
    public void test() {
        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
        People people = (People) context.getBean("people");
        people.getCat().shout();
        people.getDog().shout();

    }
}
```
### ByName
手动配置xml过程中，因为输入问题可能造成字母缺漏和大小写问题，所以这里使用自动装配的ByName
```
<bean id="people" class="com.funi.pojo.People" autowire="byName">
        <property name="name" value="qinjiang"/>
    </bean>
```
- 将查找其类中所有的set方法名，例如setCat，获得将set去掉并且首字母小写的字符串，即cat。
- 去spring容器中寻找是否有此字符串名称id的对象。
- 如果有，就取出注入；如果没有，就报空指针异常。

---
### ByType
顾名思义是通过类型来进行自动装配，但这里需要注意同一类型的对象在Spring中唯一，毕竟不唯一的话，Spring又怎么知道该装配那个呢？
```
<bean id="cat" class="com.funi.pojo.Cat"/>
<bean id="cat2" class="com.fnli.pojo.Cat"/>
```
这就是对象不唯一

同时因为是根据类型来装配，所以id的名字正不正确也就不影响了。当然ByName模式id肯定要正确，id是它恰饭的家伙嘛。
```
    <bean id="dog456" class="com.funi.pojo.Dog"/>
    <bean id="cat123" class="com.funi.pojo.Cat"/>

    <bean id="people" class="com.funi.pojo.People" autowire="byType">
        <property name="name" value="qinjiang"/>
    </bean>
```

---
### 注解
呜呼，终于要开始使用看着高大上的注解了。jkd1.5、spring2.5开始全面支持注解。

首先我们要告诉Spring我们要使用注解了（官网core 1.9）
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <context:annotation-config/>
    //用于激活那些已经在spring容器里注册过的bean上面的注解

</beans>
```
#### Autowired
- @Autowired 是按照类型匹配，不支持id,set方法也可以去除了
- 还需要导入aop包嗷~
```
<dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>5.2.0.RELEASE</version>
    </dependency>
    如果你和我导入的一样，就不用特意去导了，它帮你导入了aop包
```
让我们来注解一下
```
    <bean id="dog" class="com.funi.pojo.Dog"/>
    <bean id="cat" class="com.funi.pojo.Cat"/>
    <bean id="people" class="com.funi.pojo.People"/>
    
    public class People {
    @Autowired
    private Cat cat;
    @Autowired
    private Dog dog;
    private String name;

```
-@Autowired（required = false/true）
```
注解对象的值是否不能为空
 true为默认选项。举例就是上方name属性 我们可没有给它设置，所以你如果给它加上@Autowired，那就是妥妥的报错，但是你加上（required = false） 那就没事了。）
```

---
#### Qualifier
- @Autowired是根据类型装配，加上@Qualifier则可以byName装配
- @Qualifier不能单独使用嗷
```
    <bean id="dog" class="com.funi.pojo.Dog"/>
    <bean id="dog2" class="com.funi.pojo.Dog"/>
    <bean id="cat" class="com.funi.pojo.Cat"/>
    <bean id="cat2" class="com.funi.pojo.Cat"/>
    
    @Autowired
    @Qualifier(value = "cat2")
    private Cat cat;
    @Autowired
    @Qualifier(value = "dog2")
    private Dog dog;
    private String name;
```
#### Resource
- @Resource 有点像把Autowired和Qualifier结合起来
- @Resource如果有指定的name属性，先按照该属性进行byName装配，其次进行默认的byName装配。以上都不成功再byType
```
  @Resource(name = "cat2")
    private Cat cat;
    @Resource
    private Dog dog;
```
#### Resource 与 Autowired 比较
- @Autowired与@Resource都可以用来装配bean。都可以写在字段上，或写在setter方法上。
- @Autowired默认按类型装配（属于spring规范），默认情况下必须要求依赖对象必须存在，如果要允许null - 值，可以设置它的required属性为false，如：@Autowired(required=false) ，如果我们想使用名称装配可以结合@Qualifier注解进行使用
- @Resource（属于J2EE复返），默认按照名称进行装配，名称可以通过name属性进行指定。如果没有指定name属性，当注解写在字段上时，默认取字段名进行按照名称查找，如果注解写在setter方法上默认取属性名进行装配。 当找不到与名称匹配的bean时才按照类型进行装配。但是需要注意的是，如果name属性一旦指定，就只会按照名称进行装配。
- 它们的作用相同都是用注解方式注入对象，但执行顺序不同。@Autowired先byTyp- e，@Resource先byName。

## 注解开发
- 使用注解记得和上面一样引入Aop包，引入coantext约束和声明

### Bean的实现
我们之前都是再beans.xml中使用bean标签注入，但是实际中会直接使用注解简化。
- 配置扫描包：这些包要注入bean，spring大哥你看一下
```
@Component("people")
// 相当于配置文件中 <bean id="people" class="当前注解的类"/>
public class People {
     String name;

    @Value("fanfuli")
    //相当于配置文件中 <property name="name" value="fanfuli"/>
    public void setName(String name) {
        this.name = name;
    }

当然你也可以不使用set方法，直接在name属性上赋值就可以了
@Value(“fanfuli”)
String name；
```
### 衍生注解
- @Controller 控制器
- @Service 服务层
- @Repository 持久化层
- @Autowired byType注入
- @Qualifier byName注入

### 作用域 @scope
- singleton：默认的  单例模式 关闭工厂 销毁对象
- prototype：多例模式 关闭工厂不销毁对象，由内部回收机制回收

### XML与注解比较
- XML适用于任何场景，结构清晰。维护方便
- 注解不是自己提供的类使用不了，开发简单方便
所以xml与注解整合开发最好啦~

---

## 代理模式
啥是代理模式啊？IOC讲完了不给我说AOP，给我整活儿？
等一哈，AOP的底层机制就是动态代理，所以我们先唠叨下代理模式
### 代理模式是啥？
代理模式就是程序设计中的一种设计模式

代理模式的定义：为其他对象提供一种代理以控制对这个对象的访问。在某些情况下，一个对象不适合或者不能直接引用另一个对象，而代理对象可以在客户端和目标对象之间起到中介的作用。

### 类型
- 静态代理
- 动态代理

### 静态代理
#### 静态代理角色
- **抽象角色**：通过接口或抽象类声明真实角色实现的业务方法
- **真实角色**:实现抽象角色，定义真实角色所要实现的业务逻辑，供代理角色调用。
- **代理角色**:实现抽象角色，是真实角色的代理，通过真实角色的业务逻辑方法来实现抽象方法，并可以附加自己的操作。
- **客户**:使用代理角色来进行一些操作 

举个栗子！ vpn翻墙
```
//抽象角色 翻墙
public interface Wall {
    public void wall();
}
```
```
//真实角色 外网
public class Website implements Wall{


    @Override
    // 要通过翻墙才能给国内用户看内容
    public void wall() {
        System.out.println("想给国内用户观看的内容");
    }
}
```
```
// 代理角色 我来帮你翻墙呀~
public class Vpn implements Wall {
    private Website web;
    public Vpn(){ }

    public Vpn(Website web){
            this.web = web;
    }


    @Override
    //翻墙
    public void wall() {
        getrequeset();
        web.wall();
        setresponse();
    }
    
    //转发请求
    public void getrequeset(){
        System.out.println("把请求都给vpn，让它转发请求");
    }
    //返回内容
    public void setresponse(){
        System.out.println("ok,把外网内容给你看嗷");
    }
}
```
```
// 我们  我们一般去找VPN
public class cilent {
    public static void main(String[] args) {
       //网站想被我们访问
        Website website = new Website();、
        //Vpn帮助网站能被我们访问
        Vpn vpn = new Vpn(website);
        
        //我们去找vpn
        vpn.wall();
    }
}
```

---
#### 静态代理的优点与缺点
- 可以使得我们的真实角色更加纯粹 . 不再去关注一些公共的事情 .
- 公共的业务由代理来完成 . 实现了业务的分工 ,
- 公共业务发生扩展时变得更加集中和方便 .

缺点
- 类多了 , 多了代理类 , 工作量变大了 . 开发效率降低 .

我们即想要静态代理的优点，又不想要缺点，那么动态代理就华丽丽的出世啦~
---
### 动态代理
动态代理，你就认为它是静态代理的plus版。它和静态代理所拥有的角色是一样的。

不过都动态了，它的代理类自然是动态生成的。动态代理一般氛围两类。

#### 类别
- 基于接口的动态代理（JDK动态代理）
- 基于类的动态代理（cglib）

现在使用的比较多的是javasist来生成动态代理，我们这里先用JDK的原生代码实现。
```
Javassist是一个开源的分析、编辑和创建Java字节码的类库。是由东京工业大学的数学和计算机科学系的 Shigeru Chiba （千叶 滋）所创建的。它已加入了开放源代码JBoss 应用服务器项目，通过使用Javassist对字节码操作为JBoss实现动态"AOP"框架。
```
jdk的动态代理则需要了解两个类
- InvocationHandler 调用处理程序
```
Object invoke(Object proxy, 方法 method, Object[] args)；
//参数 
//proxy - 调用该方法的代理实例 
//method -所述方法对应于调用代理实例上的接口方法的实例。 方法对象的声明类将是该方法声明的接口，它可以是代理类继承该方法的代理接口的超级接口。 
//args -包含的方法调用传递代理实例的参数值的对象的阵列，或null如果接口方法没有参数。 原始类型的参数包含在适当的原始包装器类的实例中，例如java.lang.Integer或java.lang.Boolean 。
```
- Proxy 代理
```
Proxy提供了创建动态代理类和实例的静态方法，它也是由这些方法床架你的所有动态类的超类。

代理实例是代理类的一个实例，每个代理实例都有一个关联的调用处理程序对象，它实现了接口InvocationHandler。通过其代理接口之一的代理实例的方法调用将被分派到实例调用处理程序的invoke方法，传递代理实例

public static Object newProxyInstance(ClassLoader loader, Class[] interfaces, InvocationHandler h)

返回的是指定接口代理类的实例


loader :类加载器来定义类
interfaces：代理类实现的接口列表
h：调度方法调用的调用处理函数
```
来 我们来尝试用动态代理来改写我们之前写的vpn的静态代理
```
public class ProxyInvocationHandler implements InvocationHandler {
    //先把抽象角色拿过来，我总要知道要代理啥吧？
    private  Wall wall;

    public void setWall(Wall wall) {
        this.wall = wall;
    }

    //生成代理  注意 返回的是代理类的实例哦~
    public Object getProxy(){
      return Proxy.newProxyInstance(this.getClass().getClassLoader(),wall.getClass().getInterfaces(),this);
    }

    @Override
    // proxy : 代理类 method : 代理类的调用处理程序的方法对象.
    // 处理代理实例上的方法调用并返回结果
    public Object invoke(Object o, Method method, Object[] objects) throws Throwable {
        getrequeset();
        //反射
        Object invoke = method.invoke(wall, objects);
        setresponse();
        return invoke;
    }

    public void getrequeset(){
        System.out.println("把请求都给vpn，让它转发请求");
    }

    public void setresponse(){
        System.out.println("ok,呐呐呐~ 这就是墙外的样子哦");
    }
}

```

```
public class cilent {
    public static void main(String[] args) {
        //真实角色
        Website website = new Website();
//        Vpn vpn = new Vpn(website);
//        vpn.wall();
        //新建一个代理角色
        ProxyInvocationHandler pro = new ProxyInvocationHandler();
        //真实对象已经传入
        pro.setWall(website);
        //返回代理类的实例 也就是ProxyInvocationHandler的实例
        Wall proxy = (Wall) pro.getProxy();
        //我们现在调用的方法就会被分派到invoke方法里去
        proxy.wall();
    }

}
```
#### 动态代理的优点
- 可以使得我们的真实角色更加纯粹 . 不再去关注一些公共的事情 .
- 公共的业务由代理来完成 . 实现了业务的分工 ,
- 公共业务发生扩展时变得更加集中和方便 .
- 一个动态代理 , 一般代理某一类业务
- 一个动态代理可以代理多个类，代理的是接口！

---
## AOP
### AOP是啥？
AOP（Aspect Oriented Programming）意为：面向切面编程，通过预编译方式和运行期动态代理实现程序功能的统一维护的一种技术。

利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发的效率。

### AOP有啥用？
**提供声明式事务；允许用户自定义切面**

- 横切关注点：跨越应用程序多个模块的方法或功能。即是，与我们业务逻辑无关的，但是我们需要关注的部分，就是横切关注点。如日志 , 安全 , 缓存 , 事务等等 ....
- 切面（ASPECT）：横切关注点 被模块化 的特殊对象。即，它是一个类。
- 通知（Advice）：切面必须要完成的工作。即，它是类中的一个方法。
- 目标（Target）：被通知对象。
- 代理（Proxy）：向目标对象应用通知之后创建的对象。
- 切入点（PointCut）：切面通知 执行的 “地点”的定义。
- 连接点（JointPoint）：与切入点匹配的执行点。

**即AOP在不改变原有代码的情况下，去增加新的功能**

### 实现AOP
说那么多，反正也听不懂，我们直接来实战一哈！

使用AOP需要导入一个依赖包哦·
```
<!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.9.4</version>
</dependency>
```

---
**第一种方式** 使用Spring API接口

首先编写我们的业务接口和实现类
```
public interface UserService {

    public void add();

    public void delete();

    public void update();

    public void search();

}
```
```
public class UserServiceImpl implements UserService{

    @Override
    public void add() {
        System.out.println("增加用户");
    }

    @Override
    public void delete() {
        System.out.println("删除用户");
    }

    @Override
    public void update() {
        System.out.println("更新用户");
    }

    @Override
    public void search() {
        System.out.println("查询用户");
    }
}
```
然后去写我们的增强类 , 我们编写两个 , 一个前置增强 一个后置增强
```
public class Log implements MethodBeforeAdvice {

    //method : 要执行的目标对象的方法
    //objects : 被调用的方法的参数
    //Object : 目标对象
    @Override
    public void before(Method method, Object[] objects, Object o) throws Throwable {
        System.out.println( o.getClass().getName() + "的" + method.getName() + "方法被执行了");
    }
}
```
```
public class AfterLog implements AfterReturningAdvice {
    //returnValue 返回值
    //method被调用的方法
    //args 被调用的方法的对象的参数
    //target 被调用的目标对象
    @Override
    public void afterReturning(Object returnValue, Method method, Object[] args, Object target) throws Throwable {
        System.out.println("执行了" + target.getClass().getName()
        +"的"+method.getName()+"方法,"
        +"返回值："+returnValue);
    }
}
```
最后去spring的文件中注册 , 并实现aop切入实现 , 注意导入约束 .
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd">

    <!--注册bean-->
    <bean id="userService" class="com.funi.UserServiceImpl"/>
    <bean id="log" class="com.funi.Log"/>
    <bean id="afterLog" class="com.funi.AfterLog"/>

    <!--aop的配置-->
    <aop:config>
        <!--切入点  expression:表达式匹配要执行的方法,execution(要执行的位置 )-->
        <aop:pointcut id="pointcut" expression="execution(* com.funi.UserServiceImpl.*(..))"/>
        <!--执行环绕; advice-ref执行方法 . pointcut-ref切入点-->
        <aop:advisor advice-ref="log" pointcut-ref="pointcut"/>
        <aop:advisor advice-ref="afterLog" pointcut-ref="pointcut"/>
    </aop:config>

</beans>
```
测试
```
public class MyTest {
    @Test
    public void test(){
        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
        UserService userService = (UserService) context.getBean("userService");
        userService.search();
    }
}
```

---
**第二种方式** 自己DIY
```
@Component("diy")
public class DIY {
    public void before(){
        System.out.println("---------方法执行前---------");
    }
    public void after(){
        System.out.println("---------方法执行后---------");
    }
}
```

```
    <!-- 方式二   -->
    <aop:config>
        <aop:aspect ref="diy">
        <aop:pointcut id="pointcut" expression="execution(* com.funi.UserServiceImpl.*(..))"/>
        <aop:before pointcut-ref="pointcut" method="before"/>
        <aop:after pointcut-ref="pointcut" method="after"/>
        </aop:aspect>
    </aop:config>
```

---
**第三种方式** 注解方式
```
@Component("annotationPointcut")
@Aspect
public class AnnotationPointcut {
    @Before("execution(* com.funi.UserServiceImpl.*(..))")
    public void before(){
        System.out.println("---------方法执行前---------");
    }

    @After("execution(* com.funi.UserServiceImpl.*(..))")
    public void after(){
        System.out.println("---------方法执行后---------");
    }

    @Around("execution(* com.funi.UserServiceImpl.*(..))")
    public void around(ProceedingJoinPoint jp) throws Throwable {
        System.out.println("环绕前");
        System.out.println("签名:"+jp.getSignature());
        //执行目标方法proceed
        Object proceed = jp.proceed();
        System.out.println("环绕后");
        System.out.println(proceed);
    }
}
```
```
    <!--  第三种方式  -->
    <aop:aspectj-autoproxy/>
    
    通过aop命名空间的<aop:aspectj-autoproxy />声明自动为spring容器中那些配置@aspectJ切面的bean创建代理，织入切面。当然，spring 在内部依旧采用AnnotationAwareAspectJAutoProxyCreator进行自动代理的创建工作，但具体实现的细节已经被<aop:aspectj-autoproxy />隐藏起来了 

<aop:aspectj-autoproxy />有一个proxy-target-class属性，默认为false，表示使用jdk动态代理织入增强，当配为<aop:aspectj-autoproxy  poxy-target-class="true"/>时，表示使用CGLib动态代理技术织入增强。不过即使proxy-target-class设置为false，如果目标类没有声明接口，则spring将自动使用CGLib动态代理。
```

---
## 整合Mybatis
## Mybatis是啥
MyBatis 是一款优秀的持久层框架，它支持定制化 SQL、存储过程以及高级映射。MyBatis 避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集。MyBatis 可以使用简单的 XML 或注解来配置和映射原生类型、接口和 Java 的 POJO（Plain Old Java Objects，普通老式 Java 对象）为数据库中的记录。

好了 废话不多讲 都是冲着SSM去了 直接开始整合吧

## 整合步骤
后面再补  不方

## Spring 声明式事务
### 事务
- 事务在项目开发过程非常重要，涉及到数据的一致性的问题，不容马虎！
- 事务管理是企业级应用程序开发中必备技术，用来确保数据的完整性和一致性。
- 事务就是把一系列的动作当成一个独立的工作单元，这些动作要么全部完成，要么全部不起作用。
- 原子性（atomicity）
事务是原子性操作，由一系列动作组成，事务的原子性确保动作要么全部完成，要么完全不起作用
- 一致性（consistency）
一旦所有事务动作完成，事务就要被提交。数据和资源处于一种满足业务规则的一致性状态中
- 隔离性（isolation）
可能多个事务会同时处理相同的数据，因此每个事务都应该与其他事务隔离开来，防止数据损坏
- 持久性（durability）
事务一旦完成，无论系统发生什么错误，结果都不会受到影响。通常情况下，事务的结果被写到持久化存储器中

### Spring中的事务管理
Spring在不同的事务管理API之上定义了一个抽象层，使得开发人员不必了解底层的事务管理API就可以使用Spring的事务管理机制。Spring支持编程式事务管理和声明式的事务管理。

**编程式事务管理**
- 将事务管理代码嵌到业务方法中来控制事务的提交和回滚
- 缺点：必须在每个事务操作业务逻辑中包含额外的事务管理代码

**声明式事务管理**
- 一般情况下比编程式事务好用。
- 将事务管理代码从业务方法中分离出来，以声明的方式来实现事务管理。
- 将事务管理作为横切关注点，通过aop方法模块化。Spring中通过Spring     AOP框架支持声明式事务管理。

**使用Spring管理事务，记得头文件约束的导入：tx**
```
xmlns:tx="http://www.springframework.org/schema/tx"

http://www.springframework.org/schema/tx
http://www.springframework.org/schema/tx/spring-tx.xsd">
```
**同时需要在spring配置文件中创建相应对象**
```
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource" />
 </bean>
```
**配置好事务之后需要去配置事务的通知**
```
这里引用的是上面整合mybatis的代码 等我后面填坑

```
**Spring事务的传播特性**
- propagation_requierd：如果当前没有事务，就新建一个事务，如果已存在一个事务中，加入到这个事务中，这是最常见的选择。
- propagation_supports：支持当前事务，如果没有当前事务，就以非事务方法执行。
- propagation_mandatory：使用当前事务，如果没有当前事务，就抛出异常。
- propagation_required_new：新建事务，如果当前存在事务，把当前事务挂起。
- propagation_not_supported：以非事务方式执行操作，如果当前存在事务，就把当前事务挂起。
- propagation_never：以非事务方式执行操作，如果当前事务存在则抛出异常。
- propagation_nested：如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，则执行与propagation_required类似的操作

Spring 默认的事务传播行为是 PROPAGATION_REQUIRED，它适合于绝大多数的情况。

**为什么要配置事务？**
- 如果不配置，就需要我们手动提交控制事务；
- 事务在项目开发过程非常重要，涉及到数据的一致性的问题，不容马虎！
