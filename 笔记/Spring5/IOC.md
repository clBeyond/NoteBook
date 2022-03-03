### IOC控制反转
#### 1. IOC底层原理
   + xml解析  工厂模式  反射
  
#### 2. IOC过程
   + xml配置文件
``` java
<bean id = "dao" class = "类完整路径"></bean>
```
   + xml解析  
   ```java
   String classValue = class属性值   //xml解析
   Class clazz = Class.forName(classValue);  //反射创建对象
   return (UserDao) clazz.newInstance();
   ```

#### 3. IOC（接口）
  1. IOC思想基于IOC容器完成，IOC容器底层就是对象工厂
  2. Spring 提供IOC容器实现的两种方式：（两个接口）
   + (1) BeanFactory
      + IOC容器开发人员使用，是Spring内部使用接口，不提供给开发人员使用
      + 加载配置文件时至加载配置文件，不会创建对象，只有在使用对象时才创建
   + (2) ApplicationContext
      + BeanFactory的子接口，功能更强大，一般给开发人员使用
      + 加载文件时就把配置文件中的对象进行创建
     (3) ApplicationContext接口的两个实现类
     + FileSystemXmlApplicationContext   路径要是带盘符的绝对路径
     + ClassPathXmlApplicationContext  src里面的路径

#### 4. IOC操作Bean管理
  ##### 1. Bean管理（两个操作）
   （1） Spring创建对象
   （2） Spring注入属性
 #####  2. Bean管理操作的两种实现方式
1.  基于XML配置文件方式实现
   + 基于XML方式创建对象
  
   ``` java
  <bean id = "user" class = "com.company.User"></bean>
   ```
   （1） 在Spring配置文件中，使用bean标签，标签里添加对应的属性，就可以实现对象创建
   （2） 在bean标签里，有很多属性，介绍常用属性
      id 属性 ： 给创建的对象起个别名，唯一标识
      class 属性 ： 类全路径（包类路径）
      name ： 和id作用差不多
   （3）创建对象时，默认调用对象的无参构造器（一定要有无参构造器）
   + 基于XML方式注入属性
   （1） DI ： 依赖注入，就是注入属性
   第一种：使用set方法注入
```java
   <bean id = "book" class = "com.company.Book">
        <!--使用property标签注入属性
        name 属性就是类里的属性名，value是要注入的属性的值
        -->
        <property name="bname" value="天龙八部"></property>

    </bean>
    测试：
        //加载配置文件
     ApplicationContext context = new ClassPathXmlApplicationContext("bean1.xml");
        //  获取配置所创建的对象
       
        Book book = context.getBean("book", Book.class);
        System.out.println(book);
```

   第二种：使用有参构造器
```java
    <bean id = "orders" class = "com.company.Orders">
        <constructor-arg name = "oname" value = "abc" ></constructor-arg>
        <constructor-arg name = "address" value="zhongguo"></constructor-arg>
    </bean>
```
   第三种：p名称空间注入，基于简化基于XML配置方式
```java
//添加p名称空间在配置文件中
xmlns:p="http://www.springframework.org/schema/p"
// 进行属性注入，在bean标签里进行
    <bean id = "book" class = "com.company.Book" p:bname="三国">
```
##### IOC操作Bean管理（XML注入其他类型属性）
   (1) 字面量（默认属性值）
   （1）null值
```java
<property name="bname" >  <!--这里不写value=“。。”  而是添加一个<null>标签-->
<null></null>
</property>
```
   （2）属性值包含特殊符号
    用转义符  &lt;  &gt;  <  >
    用<![CDATA[]]>
```java
<property name="bname" > 
<value><![CDATA[这里放特殊字符]]></value>
</property>
```
（2）注入属性-外部bean
+ 创建两个类Service 和 dao
+ 在service调用dao里面的方法
+ 在Spring配置文件中进行配置
```java
    <bean id = "userService" class = "com.company.service.UserService">
<!--    注入userDao 对象-->
<!--        name属性：类里面属性名称-->
<!--        ref属性：创建userDao对象bean标签id值-->
        <property name="userDao" ref = "userDaoImpl"></property>
    </bean>
    <bean id = "userDaoImpl" class="com.company.dao.UserDaoDemo"></bean>
```
（3）注入属性-内部bean和级联赋值
1. 基于注解方式实现


