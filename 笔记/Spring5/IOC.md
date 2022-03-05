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
（1） 一对多关系：部门和员工的关系
```java
//内部bean
 <bean id = "emp " class = "com.company.bean.Emp">
        <property name="ename"  value="tom"></property>
        <property name="gender"  value = "女"></property>
        <property name="dept" >
            <bean id = "dept_sales" class = "com.company.bean.Dept">  //内部嵌套
                <property name="name"  value = "销售"></property>
            </bean>
        </property>
        
    </bean>
```
```java
//级联赋值（1）
 <bean id = "emp" class = "com.company.bean.Emp">
        <property name="ename"  value="tom"></property>
        <property name="gender"  value = "女"></property>
        <property name="dept" ref = "dept"></property>
    </bean>
    <bean id = "dept" class="com.company.bean.Dept">  //在创建外部bean时注入值
        <property name="name" value="财务"></property>
    </bean>
```
```java
<bean id = "emp" class = "com.company.bean.Emp">
        <property name="ename"  value="tom"></property>
        <property name="gender"  value = "女"></property>
        <property name="dept" ref = "dept"></property>
        <property name = "dept.dname" value = "技术部"> </property>  //这种写法要在Emp中写出dept的get方法
    </bean>
    <bean id = "dept" class="com.company.bean.Dept">  
        <property name="name" value="财务"></property>
    </bean>
```

##### IOC操作Bean管理（XML注入集合属性）
1. 注入数组/List/Map/Set属性
```java
 <bean id = "stu" class="com.tongji.collectiontype.Stu">
        <property name="course" >
            <array>
                <value>java</value>
                <value>python</value>
            </array>
        </property>
        <property name="list">
            <list>
                <value>小红</value>
                <value>小明</value>
            </list>
        </property>
        <property name="maps">
            <map>
                <entry key="1" value="90"></entry>
                <entry key="2" value="89"></entry>
            </map>
        </property>
        <property name="set">
            <set>
                <value>mysql</value>
                <value>radis</value>
            </set>
        </property>
    </bean>
```  
2. 在集合中设置对象类型
```java
<property name="courseList">
            <list>
                <ref bean="course1"></ref>
                <ref bean="course2"></ref>
                <ref bean="course3"></ref>
            </list>
        </property>
    </bean>
    <bean id = "course1" class="com.tongji.collectiontype.Course">
        <property name="cname" value="mysql"></property>
    </bean>
    <bean id = "course2" class="com.tongji.collectiontype.Course">
        <property name="cname" value="spring"></property>
    </bean>
    <bean id = "course3" class="com.tongji.collectiontype.Course">
        <property name="cname" value="python"></property>
    </bean>
```
3. 把集合注入部分提取出来
```java
//在Spring配置文件中引入名称空间util
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:util="http://www.springframework.org/schema/util"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util.xsd">


    
</beans>
//使用util标签完成list集合注入提取
<util:list id = "bookList">
        <value>天龙</value>
        <value>倚天</value>
    </util:list>

    <bean id = "book" class="com.tongji.collectiontype.Book">
        <property name="list" ref="bookList"></property>
    </bean>
```

##### IOC操作bean管理（FactoryBean）
普通bean和工厂bean
+ 普通bean ： 在配置文件中定义的bean的类型就是返回类型
+ 工厂bean ： 在配置文件中定义的bean类型可以和返回类型不同

第一步 ： 创建类，让这个类作为工厂bean，实现接口factorybean
第二步 ： 实现接口里的方法，在实现的方法中返回bean类型
```java
public class MyBean implements FactoryBean {  //实现接口

    @Override
    public Object getObject() throws Exception {  //在这里设置你想返回的类型
        return null;
    }

    @Override
    public Class<?> getObjectType() {
        return null;
    }

    @Override
    public boolean isSingleton() {
        return FactoryBean.super.isSingleton();
    }
}
```

##### IOC操作bean管理（bean的作用域）
1. 在Spring里面，设置创建bean实例是多实例还是单实例
2. 在Spring里面，默认情况下，bean是单实例 ，即多次调用getBean获得的是同一个对象
3. 如何设置为多实例对象
   通过bean标签里scope属性来设置
   scope值 ： 1 默认 singleton 单实例  2 prototype 多实例
```java
    <bean id = "stu" class="com.tongji.collectiontype.Stu" scope="prototype">
```
   设置为单实例时，在加载配置文件时就会创建对象，多实例时，只有在调用getBean方法时才创建对象

##### IOC操作bean管理（bean的生命周期）
1. 生命周期
   对象从创建到销毁的过程
2. bean生命周期
   （1）通过构造器创建bean实例（无参构造）
   （2）为bean的属性设置值和对其他bean的引用（调用set方法）
   （3）调用bean的初始化方法（需要配置初始化方法 <bean init-method = " "）
   （4）bean可以使用了（对象获取到了）
   （5）当容器关闭时，调用bean的销毁方法（需要进行配置销毁的方法 destroy-method = " "）
3. 生命周期后置处理器
   （1）通过构造器创建bean实例（无参构造）
   （2）为bean的属性设置值和对其他bean的引用（调用set方法）
   （3-1）把bean实例传递bean后置处理器的方法
   （3-2）调用bean的初始化方法（需要配置初始化方法 <bean init-method = " "）
   （3-3）把bean实例传递bean后置处理器的方法
   （4）bean可以使用了（对象获取到了）
   （5）当容器关闭时，调用bean的销毁方法（需要进行配置销毁的方法 destroy-method = " "）
 
##### IOC操作bean管理（xml自动装配）
bean标签属性autowire配置自动装配
autowire有两个值
（1） byName 根据属性名称注入，注入值bean的id值和类属性名称必须一致
```java 
<bean id = "emp" class="com.tongji.autowire.Emp" autowire="byName"> </bean>
    <bean id = "dept" class="com.tongji.autowire.Dept"></bean>
```
（2） byType 根据属性类型注入
此时相同类型的bean只能定义一个
```java 
<bean id = "emp" class="com.tongji.autowire.Emp" autowire="byType"> </bean>
    <bean id = "dept" class="com.tongji.autowire.Dept"></bean>
```

##### IOC操作Bean管理（引入外部属性文件）
```java
//引入context名称空间
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                          http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
```

```java
//  引入文件
   <context:property-placeholder location="classpath:jdbc.properties"/>
        <bean id = "dataSource" class = "com.alibaba.druid.pool.DruidDataSource">
            <property name="driverClassName" value="${prop.driverClass}"></property>
            <property name="url" value="${prop.url}"></property>
            <property name="username" value="${prop.userName}"></property>
            <property name="password" value="${prop.password}"></property>
        </bean>
```
```java
//jdbc.properties文件内容
prop.driverClass=com.mysql.cj.jdbc.Driver
prop.url=jdbc:mysql://localhost:3306.userDb
prop.userName=root
prop.password=Cl2133030*
```

##### IOC管理（基于注解方式）
1. 什么是注解
   （1）注解是代码特殊标记，格式：@注解名（属性名称=属性值，属性名称=属性值）
   （2）使用注解，注解作用在类上面，方法上面，属性上面
   （3）使用注解的目的：简化XML文件配置
2. Bean管理提供的注解
   （1）@Component
   （2）@Service
   （3）@Controller
   （4）@Repository
   上面四个注解功能一样，只是为了开发更清晰，不同注解用在不同层
3. 基于注解方式创建对象
   （1）引入依赖
   （2）开启组件扫描
        引入context名称空间
```java
        <context:component-scan base-package="com.zhujie"></context:component-scan>
        ```
        用context:component-scan标签指出要扫描的包的路径，若有多个包，可用，隔开，或者写他们上层目录
```java
   @Component (value="userService")   //相当于<bean id="userService" class="">
   // value 可以不写，默认为类名首字母小写
   public class UserService {
      public void add(){
          System.out.println("service add()...");
    }
}
```
 开启组件扫描的细节
```java
//只扫描Component注解
<context:component-scan base-package="com.zhujie" use-default-filters="false">
        <context:include-filter type="annotation" expression="org.springframework.stereotype.Component"/>
    </context:component-scan>
```
```java
//不扫描Component注解
<context:component-scan base-package="com.zhujie" use-default-filters="false">
        <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Component"/>
    </context:component-scan>
```
4. 基于注解方式的属性注入
   (1)@AutoWired：根据属性类型进行自动装配
```java
public class UserService {
    @Autowired
    private UserDao userDao;
    public void add(){
        System.out.println("service add()...");
        userDao.add();
    }
}
```
   (2)@Qualifier：根据属性名进行注入
   和Auto Wired一起使用，因为根据属性类型自动装配可能会出现一个接口多个实现类不知道匹配哪个的问题
```java
@Component (value="userService")   //<bean id="userService" class="">
// value 可以不写，你认为类名首字母小写
public class UserService {
    @Autowired
    @Qualifier(value = "userService")
    private UserDao userDao;
    public void add(){
        System.out.println("service add()...");
        userDao.add();
    }
}
```
   (3)@Resource：可以根据类型注入，也可以根据名称注入
   @Resource(name = "userService")
   @Resource
   (4)@Value :注入普通类型属性
```java
public class UserService {
    @Value(value = "abc")
    private String name;
    @Autowired
    private UserDao userDao;
    public void add(){
        System.out.println(this.name);
        userDao.add();
    }
}
```

5. 纯注解开发
   （1）创建配置类，替代XML配置文件