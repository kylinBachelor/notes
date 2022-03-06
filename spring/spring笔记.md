# 1. spring基本概念
## spring容器
&emsp;&emsp;spring容器的概念，容器这个名字起的非常好，容器内可以放置很多东西，在我们程序启动的时候就会创建spring容器，程序会给spring容器一个清单，清单中列出了需要创建的对象以及依赖关系，spring容器会创建和组装好清单中的对象，然后将这些对象放在spring 容器中，，当程序中需要使用的时候，可以到容器中查找获取，然后直接使用。
## IOC 控制反转
&emsp;&emsp;使用者之前需要一个对象的时候都需要自己去创建和组装所依赖的对象，然而现在这些创建和组装都交给了spring容器去做，使用者只需要去spring容器中查找需要使用的对象就可以了；在这个过程中对象的创建和组装被反转了，之前是使用者自己去组装和创建的，现在交给spring容器去创建和组装了，对象的构建过程被反转了，所以叫做控制反转；IOC是面向对象编程中的一种设计原则，主要是为了降低系统代码的耦合度，让系统更加有利于去维护和扩展。
## DI依赖注入
&emsp;&emsp;依赖注入是spring容器中创建对象时给其设置依赖对象的方式，比如spring给了一个清单，清单中列出了需要创建B对象以及其它的一些对象（可能包含了B对象中依赖的对象），此时spring在创建B对象时会看B对象依赖哪些对象，然后去清单中查找有没有包含这些被依赖的对象，如果有就去将其创建好，然后将其创建给B对象；可能B需要依赖很多对象，B创建之前完全不需要知道其他对象是否存在或者其它对象在哪里以及被他们是如何创建的，而spring容器会将B依赖对象主动创建好并将其注入到B中去，比如spring容器创建B的时候，发现B需要依赖A，那么spring容器在清单中找到A的定义并将其创建好之后，注入到B对象中。
## 总结
1. IOC控制反转：它是一种设计理念，将对象的创建和组装的主动权交给了spring容器去做，控制的动作被反转了，降低了系统的耦合度，有利于系统的维护和扩展，主要就是指需要使用的对象的组装的控制权被反转了，之前是自己要做的，现在交给spring容器去做了。
2. DI依赖注入：表示spring容器中创建对象时给其设置依赖的方式，通过某些注入方式可以让系统更加灵活，比如自动注入等可以让系统变得很灵活，
3. spring容器：主要负责容器中对象的创建、组装、对象查找、对象生命周期的管理等等操作。

# 2. Spring容器的基本使用及原理
## IOC容器
&emsp;&emsp;IOC容器是具有依赖注入功能的容器，负责**对象的实例化，对象的初始化，对象和对象之间依赖关系配置，对象那个的销毁，对外提供对象的查找**等操作，对象的整个生命周期都是通过容器来控制的。我们需要使用的对象都由IOC容器进行管理，不需要我们再动手去new的方式创建对象，由IOC容器直接帮我们组装好，当我们需要使用的时候直接从IOC容器中获取就可以了。  
  **那么spring Ioc是如何知道需要管理哪些对象的呢？**  
  &emsp;&emsp;需要我们提供给spring一个配置的清单，这个配置支持xml格式和Java注解的方式，在配置文件中列出需要让IOC容器管理的对象，以及可以指定让IOC容器如何构建这些对象，当spring容器启动的时候，就回去加载这个配置文件，然后将这些对象组装好以供外部的访问者使用。  
  这里所说的IOC容器也叫做spring容器
  ## Bean的概念
  &emsp;&emsp;由spring容器管理的对象叫做Bean对象。Bean就是普通的Java对象，和我们自己new的对象其实是一样的，只是这些对象是由spring去创建和管理，我们需要在配置文件中告诉spring容器需要创建那些Bean对象，所以需要先在配置文件中告诉spring容器需要创建哪些Bean对象，所以需要先在配置文件中定义好需要创建的bean对象，这些配置统称为bean定义配置元数据信息，spring容器通过读取这些bean配置元数据信息，来构建和组装我们需要的对象。
  
## Spring容器使用步骤
  1. 引入spring相关的maven配置
  2. 创建bean配置文件，比如bean.xml配置文件
  3. 在bean xml文件中定义好需要spring容器管理的bean对象
  4. 创建spring容器，并给容器指定需要装载的bean配置文件，当spring容器启动之后，会加载这些配置文件，然后创建好配置文件中定义好的bean对象，将这些对象放在容器中以供使用。
  5. 通过容器提供的方法获取容器中的对象，然后使用

## Spring容器对象
&emsp;&emsp; spring内部提供了很多表示spring容器接口的对象，下面看看比较常见的几个容器接口和具体的实现类。
### BeanFactory接口
```java
org.springframework.beans.factory.BeanFactory
```
  &emsp;&emsp;spring容器中具有代表性的容器就是BeanFacotry接口了，这个是spring容器的顶层接口，提供了容器最基本的功能。
### 常用的几个方法
```java
//按bean的id或者别名查找容器中的bean
Object getBean(String name) throws BeansException
//这个是一个泛型方法，按照bean的id或者别名查找指定类型的bean，返回指定类型的bean对象
<T> T getBean(String name, Class<T> requiredType) throws BeansException;
//返回容器中指定类型的bean对象
<T> T getBean(Class<T> requiredType) throws BeansException;
//获取指定类型bean对象的获取器，这个方法比较特别，以后会专门来讲
<T> ObjectProvider<T> getBeanProvider(Class<T> requiredType);
```
### ApplicationContext接口
```java
org.springframework.context.ApplicationContext
```
&emsp;&emsp; 这个接口继承了BeanFactory接口，所以内部包含了BeanFactory的所有功能，并且在其上进行了扩展，增加了很多企业级功能，比如AOP、国际化、事件支持......
### ClassPathXmlApplicationContext类
```java 
org.springframework.context.support.ClassPathXmlContext
```
&emsp;&emsp;这个类实现了ApplicationContext接口，注意一下这个类包含了ClassPathXml,说明这个容器类可以从classpath中加载bean.xml配置文件，然后创建xml中配置的bean对象。
### AnnotationConfigApplicationContext类
```java
org.springframework.context.annotation.AnnotationConfigApplicationContext
```
&emsp;&emsp;这个类也实现了ApplicationContext接口，注意其类名包含了Annotation和Config这两个单词，上面我们说过，bean的定义支持xml的方式和注解方式，当我们使用注解的方式定义bean的时候，就需要用到这个容器来装载了，这个容器内部会解析注解来构建和管理需要的bean。  
注解的方式相对于xml方式更方便一些，也是比较推荐的方式（也是一个趋势，比如springboot）

# 3. xml中bean定义详解
## bean概念
&emsp;&emsp;被spring管理的对象统称为bean，我们程序中需要用到很多对象，我们将这些对象让spring去帮我们创建和管理，我们可以通过bean xml配置文件告诉spring容器需要管理哪些bean,spring帮我们创建和组装好这些bean对象，spring内部将这些名称和具体的bean对象进行绑定，然后spring容器可以通过这个的名称找对我们需要的对象，这个名称叫做bean名称，在一个spring容器中需要是唯一的。
## bean xml配置文件格式
bean xml文件用于定义spring容器需要管理的bean,常见的格式如下：
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-4.3.xsd">
	
	<import resource="引入其他bean xml配置文件" />
	<bean id="bean标识" class="完整类型名称"/>
	<alias name="bean标识" alias="别名" />
</beans>
```
&emsp;&emsp;beans是根元素，下面可以包含任意数量的import、bean、alias元素，下面我们对每个元素进行详解。
## bean元素
用来定义一个bean对象
## 格式
```xml
<bean id="bean唯一标识" name="bean名称" class="完整类型名称" factory-bean="工厂bean名称" factory-method="工厂方法"/>
```
## bean名称
&emsp;&emsp;每个bean都有一个名称,叫做bean名称，bean名称在一个spring容器中必须是唯一的，否则会报错，通过bean名称可以从spring容器中获取对应的bean对象。
## bean别名
&emsp;&emsp;别名： 相当于人的外号一样，一个人可以有很多外号，当别人喊这个人的名称和外号的时候，都可以找到这个人。那么bean也是一样，也可以给bean起几个外号,这个外号在bean中叫做bean的别名，spring容器允许使用者通过名称或者别名获取对应的bean对象。
## bean名称别名定义规则
名称和别名可以通过bean元素中的id和name来定义，具体定义规则如下：
1. 当id存在的时候，不管name有没有，取id为bean的名称。
2. 当id不存在的时候，此时需要看name，name的值可以通过 , ; 或空格 分隔，最后会按照分隔符得到一个String数组，数组的第一个元素作为bean的名称，其它的作为bean的别名
3. 当id和name 都存在的时候，id为bean的名称，name 用来定义多个别名
4. 当id和name都不指定的时候，bean名称自动生成，生成规则下面详细说明
## 案例
下面演示一下bean名称和别名的各种写法。
```xml
<!-- 通过id定义bean名称:user1-->
<bean id="user1" class="com.example.UserModel/>

<!-- 通过name定义bean名称：user2-->
<bean name="user2" class="com.example.UserModel"/>

<!-- id为名称，name为别名；bean名称：user3,1个别名：user3_1-->
<bean name="user3" name="user3_1" class="com.example.UserModel"/>

<!-- id为名称：user4, 多个别名：user4_1,user4_2,user4_3,user4_4 -->
<bean id="user4" name="user4_1,user4_2;user4_3 user4_4" class="com.example.UserModel"/>

<!-- bean名称：user5, 别名：user5_1,user5_2,user5_3,user5_4 -->
<bean name="user5,user5_1,user5_2,user5_3,user5_4" class="com.example.UserModel">
```
测试用例如下：
```java
public class Example {
  public static void main(String[] args) {
    //1.bean配置文件位置
    String beanXml = "classpath:/com/example/beans.xml";
    //2.创建ClassPathXmlApplicationContext容器，给容器指定需要加载的bean配置文件
    ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext(beanXml);
    for (String beanName : Arrays.asList("user1", "user2", "user3", "user4","user5")) {
      //获取bean的别名
      String[] aliases = context.getAliases(beanName);
      System.out.println(String.format("beanName:%s,别名:[%s]", beanName,String.join(",", aliases)));
    }
    System.out.println("spring容器中所有bean如下：");
    //getBeanDefinitionNames用于获取容器中所有bean的名称
    for (String beanName : context.getBeanDefinitionNames()) {
      //获取bean的别名
      String[] aliases = context.getAliases(beanName);
      System.out.println(String.format("beanName:%s,别名:[%s]", beanName,String.join(",", aliases)));
    }
  }
}
```
上面有两个新方法：
1. getAliases: 通过bean名称来获取这个bean的所有别名
2. getBeanDefinitionNames: 返回spring容器中定义的所有bean的名称
## id和name都未指定
当id和name都未指定的时候，bean的名称和别名此时由spring自动生成，bean名称为
```xml
<!-- bean的class的完整类名#编号 -->
<!-- 编号是从0开始的同种类型的没有定义名称的一次递增 -->
```
## alias元素
alias元素也可以用来给某个bean定义别名，语法：
```xml
<alias name="需要定义的别名bean" alias="别名"/>
<!-- 例如 -->
<bean id="user6" class="com.example.user6"/>
<alias name="user6" alias="user6_1"/>
<alias name="user6" alias="user6_2"/>
```
## import元素
&emsp;&emsp;当我们的系统比较大的时候，会被分成很多模块，每个模块会对应一个bean xml 文件，我们可以在一个总的bean xml中对其它bean xml 进行汇总，相当于把多个bean xml的内容合并到一个里面了，可以通过import元素引入其它bean配置文件。  
语法：
```xml
<import resource="其它配置文件的位置"/>
```
例如：
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-4.3.xsd">
  <import resource="user.xml" />
  <import resource="order.xml" />
</beans>
```
# 4. 容器创建bean实例有多少种
## 简述
1. 通过反射调用构造方法创建bean对象
2. 通过静态工厂方法创建bean对象
3. 通过实例工厂方法创建bean对象
4. 通过FactoryBean创建bean对象
## 1. 通过反射调用构造方法创建bean对象
调用类的构造方法获取对应的bean实例，是使用最多的方式，这种方式只需要在xml bean元素中指定class属性，spring容器内会自动调用该类型的构造方法来创建bean对象，将其放在容器中以供使用。
### 语法
```xml
<bean id="bean名称" name="bean名称或别名" class="bean的完整类型名称">
  <constructor-arg index="0" value="bean的值" ref="引用的bean名称" />
  <constructor-arg index="1" value="bean的值" ref="引用的bean名称" />
  <constructor-arg index="2" value="bean的值" ref="引用的bean名称" />
  <constructor-arg index="3" value="bean的值" ref="引用的bean名称" />
  ......
  <constructor-arg index="4" value="bean的值" ref="引用的bean名称" />
</bean>
```
```text
constructor-arg：用于指定构造方法参数的值。
index: 构造方法中参数的位置，从0开始依次递增。
value: 指定参数的值。
ref: 当插入的值为容器内其它bean的时候，这个值为容器中对应bean的名称。
```
## 2. 通过静态工厂方法创建bean对象
我们可以创建静态工厂，内部提供一些静态方法来生产所需的对象，将这些静态方法创建的对象交给spring以供使用。  
### 语法：
```xml
<bean id="bean名称" name="" class="静态工厂完整类名" factory-method="静态工厂方法">
  <constructor-arg index="0" value="bean的值" ref="引用的bean名称" />
  <constructor-arg index="1" value="bean的值" ref="引用的bean名称" />
  <constructor-arg index="2" value="bean的值" ref="引用的bean名称" />
  <constructor-arg index="3" value="bean的值" ref="引用的bean名称" />
</bean>
```
```text
class: 指定静态工厂完整的类名
factory-method: 静态工厂中的静态方法，返回需要的对象。
constructor-arg: 用于指定静态方法参数的值，用法和上面介绍的构造方法一样
```
spring容器会自动调用静态工厂的静态方法获取指定的对象，将其放在容器中以供使用。
## 3. 通过实例工厂方法创建bean对象
让spring容器去调用某些对象的某些实例方法来生成bean对象放在容器中以供使用。  
### 语法：
```xml
<bean id="bean名称" factory-bean="需要调用的实例对象bean名称" factory-method="bean对象中的方法">
  <constructor-arg index="0" value="bean的值" ref="引用的bean名称"></constructor-arg>
  <constructor-arg index="1" value="bean的值" ref="引用的bean名称"></constructor-arg>
  <constructor-arg index="2" value="bean的值" ref="引用的bean名称"></constructor-arg>
  <constructor-arg index="3" value="bean的值" ref="引用的bean名称"></constructor-arg>
  <constructor-arg index="4" value="bean的值" ref="引用的bean名称"></constructor-arg>
</bean>
```
spring容器以factory-bean的值为bean名称查找对应的bean对象，然后调用该对象中factory-method属性值指定的方法，将这个方法返回的对象作为当前的bean对象放在容器中以供使用。
## 4. 通过FactoryBean来创建bean对象
前面我们说到了BeanFactory接口，BeanFactory是spring容器的顶层接口，而这里说的是FactoryBean也是一个接口，这两个接口很容易混淆，FactoryBean可以让spring容器通过这个接口的实现来创建我们需要的bean对象。  
### FactoryBean接口源码：
```java
public interface FactoryBean<T> {
  /**
  * 返回创建好的对象
  */
  @Nullable
  T getObject() throws Exception;
  /**
  * 返回需要创建的对象的类型
  */
  @Nullable
  Class<?> getObjectType();
  /**
  * bean是否是单例的
  */
  default boolean isSingleton() {
    return true;
  }
}

```
接口中有三个方法，前面两个方法需要我们去实现，getObject方法内部由开发者自己去实现对象的创建，然后将创建好的对象返回给spring容器，getObjectType需要指定我们创建的bean类型；最后一个方法isSingleton表示通过这个接口创建的对象是否是单例的，如果返回false,那么每次从容器中获取对象的时候都会调用这个接口的getObject()去生成bean对象。  
语法：  
```xml
<bean id="bean名称" class="FactoryBean接口实现类" />
```
### 案例：
```java
public class UserFactoryBean implements FactoryBean<UserModel> {

  int count = 1;

  @Nullable
  @Override
  public UserModel getObject() throws Exception { // @1
    UserModel userModel = new UserModel();
    userModel.setName("我是通过FactoryBean创建的第" + count + "对象"); // @4
    return userModel;
  }

  @Nullable
  @Override
  public Class<?> getObjectType() {
    return UserModel.class; //@2
  }

  @Override
  public boolean isSingleton() {
    return true; //@3
  }
}
```
```text
@1: 返回了一个创建好的UserModel对象
@2: 返回对象的class对象
@3：返回true，表示创建的对象是单例的，那么我们每次从容器中获取这个对象的时候都是同一个对象
@4：此处用了一个count，通过这个可以看出isSingleton不同返回值的时候从容器获取bean是否是同一个
```
### bean xml配置
```xml
<!-- 通过FactoryBean 创建UserModel对象 -->
<bean id="createByFactoryBean" class="com.example.UserFactoryBean"/>
```
# 5. bean作用域scope详解
&emsp;&emsp;应用中，有时候我们需要一个对象在整个应用中只有一个，有些对象希望每次使用的时候都重新创建一个，spring对我们这种需求也提供了支持，在spring中这个叫做bean的作用域
```xml
<bean id="" class="" scope="作用域"></bean>
```
spring容器中scope常见的有五种。
## singleton
&emsp;&emsp;当scope的值设置为singleton的时候，整个spring容器只会存在一个bean实例，通过容器多次查找bean的时候（调用BeanFactory的getBean方法或者bean之间注入依赖的bean对象的时候），返回的都是同一个bean对象，singleton是scope的默认值，所以spring容器中默认创建的bean对象是单例的，通常spring容器在启动的时候，会将scope为singleton的bean创建好放在容器中（有个特殊情况，当bean的lazy为true的时候，表示懒加载，需要使用的时候才会创建），用的时候直接返回。
### 案例
### bean xml配置
```xml
<bean id="" name="" class="" scope="singleton">
  <constructor-arg index="" value=""></constructor-arg>
</bean>
```
### 单例bean使用注意
**单例bean是整个应用共享的，所以需要考虑到线程安全的问题，springMVC中controller默认是单例的，有些开发者在controller中创建了一些变量，那么这些变量实际上就变成共享的了，controller可能会被很多线程同时访问,这些线程并发去修改controller中的共享变量，可能会出现数据错乱的问题；所以使用的时候需要特别注意！！！！！！！！**
## prototype
&emsp;&emsp;如果scope被设置成prototype类型了，表示这个bean是多例的，通过容器每次获取的bean都是不同的实例，每次获取都会重新创建一个bean实例对象。
### 案例
### bean xml配置
```xml
<bean id="" name="" class="" scope="prototype">
  <constructor-arg index="" value=""></constructor-arg>
</bean>
```
### 说明
&emsp;&emsp;容器启动过程中并没有去创建多例的对象，每次获取的bean得到的都是不同的实例，每次获取的时候才会调用构造方法创建bean实例
### 多例bean使用注意
**&emsp;&emsp;多例bean每次获取的时候都会重新创建，如果这个bean比较复杂，创建的事件会比较长，会影响系统的性能。**  
**下面介绍的三个：request,session,application都是springWeb环境中才有的**
## request
&emsp;&emsp;当一个bean的作用域为request时，表示在一次http请求中，一个bean对应一个实例；对每个http请求都会创建一个bean实例，request结束的时候，这个bean也就结束了，request作用域用在spring容器的web环境中，这个以后讲springMVC的时候就会说，spring中有个web容器接口WebApplicationContext,这个里面对request作用域提供了支持，配置方式：
```xml
<bean id="" name="" scope="request"></bean>
```
## session
&emsp;&emsp;这个和request类似，也是用在web环境中，session级别共享的bean，每个会话会对应一个bean实例，不同的session对应不同的bean实例，springMVC中再细说。
```xml
<bean id=" " class="" scope="session"></bean>
```
## Application
&emsp;&emsp;全局web应用级别的作用域，也是在web环境中使用的，一个web应用程序对应一个bean实例,通常情况下和singleton效果类似，不过也有不一样的地方，singleton是每个spring容器中只有一个bean实例，一般我们的程序只有一个spring容器，但是，一个应用程序可以创建多个spring容器，不同的容器中可以存在同名的bean,但是scope==application的时候，不管应用中有多少个容器，这个应用中同名的bean只有一个。
```xml
<bean id="" name="" class="" scope="application"></bean>
```
## 自定义scope
有时候，spring内置的集中scope不能满足我们的要求的时候，我们可以自定义bean的作用域
## 自定义scope三步骤
### 第一步实现scope接口
接口如下：
```java
import org.springframework.beans.factory.ObjectFactory;
import org.springframework.lang.Nullable;
public interface Scope {
  /**
  * 返回当前作用域中name对应的bean对象
  * name：需要检索的bean的名称
  * objectFactory：如果name对应的bean在当前作用域中没有找到，那么可以调用这个
  ObjectFactory来创建这个对象
  **/
  Object get(String name, ObjectFactory<?> objectFactory);
  /**
  * 将name对应的bean从当前作用域中移除
  **/
  @Nullable
  Object remove(String name);
  /**
  * 用于注册销毁回调，如果想要销毁相应的对象,则由Spring容器注册相应的销毁回调，而由自定义作
  用域选择是不是要销毁相应的对象
  */
  void registerDestructionCallback(String name, Runnable callback);
  /**
  * 用于解析相应的上下文数据，比如request作用域将返回request中的属性。
  */
  @Nullable
  Object resolveContextualObject(String key);
  /**
  * 作用域的会话标识，比如session作用域将是sessionId
  */
  @Nullable
  String getConversationId();
}
```
### 第二部 将自定义的scope注册到容器
&emsp;&emsp;需要调用org.springframework.beans.factory.ConfigurableBeanFactory#registerScope的方法，看一下这个方法的声明。
```java
/**
* 向容器中注册自定义的Scope
*scopeName：作用域名称
* scope：作用域对象
**/
void registerScope(String scopeName, Scope scope);
```
### 第三步：使用自定义的作用域
定义bean的时候，指定bean的scope属性为自定义的作用域名称。
### 案例
**需求**  
&emsp;&emsp;下面我们来实现一个线程级别的bean作用域，同一个线程中同名的bean是同一个实例，不同的线程中的bean是不同的实例。
### 实现分析
&emsp;&emsp;需求中要求bean在线程中是贡献的，所以我们可以通过ThreadLocal来实现，ThreadLocal可以实现线程中数据的共享。
#### ThreadScope
```java
// 自定义本地线程级别的bean作用域，不同的线程中对应的bean实例是不同的，同一个线程中同名的bean是一个实例
public class ThreadScope implements Scope{
  public static final String THREAD_SCOPE = "thread";  // @1
  private ThreadLocal<Map<String,Object>> beanMap = new ThreadLocal() {

    @Override
    protected Object initValue() {
      reutrn new HahsMap<>();
    }

    @Override
    public Object get(String name,ObjectFactory<?> objectFactory) {
      Object bean = beanMap.get().get(name);
      if (Objects.isNull(bean)) {
        bean = objectFactory.getObject();
        beanMap.get().put(name,bean);
      }
      return bean;
    }

    @Nullable
    @Override
    public Object remove(String name) {
      return this.beanMap.get().remove(name);

    }

    @Override
    public void registerDestructionCallback(String name, Runnable callback) {
      //bean作用域范围结束的时候调用的方法，用于清理bean
      System.out.println(name);
    }

    @Nullable
    @Override
    public Object resolveContextualObject(String key) {
      return null;
    }

    @Nullable
    @Override
    public String getConversationId() {
      return Thread.currentThread().getName();
    }
  }
}

```
```text
@1: 定义了作用域的名称为一个常量thread，可以在定义bean的时候给scope使用
```
#### BeanScopeModel
```java
public class BeanScopeModel{
  public BeanScopeModel(String BeanScope) {
    System.out.println(String.format("线程：%s,create BeanScopeModel,{scope=%s},{this=%s}", Thread.currentThread(), beanScope, this))
  }
}
```
上面的构造方法中会输出当前线程的信息，到时候可以看到创建bean的线程。
#### bean的配置文件
beans-thread.xml 的内容
```xml
<?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-4.3.xsd">
  <!-- 自定义scope的bean -->
  <bean id="threadBean" class="com.example.BeanScopeModel" scope="thread">
    <constructor-arg index="0" value="thread"/>
  </bean>
</beans>
<!-- 上面的scope是我们自己定义的，值为thread -->
```
#### 测试用例
```java
public class ThradScopeTest{
  public static void main(String[] args) throws InterruptedException {
    // 配置文件位置
    String beanXml = "classpath:/com/example/beans-thread.xml";
    // 手动创建容器
    ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext(){
      @Override
      protected void postProcessBeanFactory(ConfigurableListableBeanFactory beanFactory) {
        // 向容器中注册自定义的scope
        beanFactory.registerScope(ThreadScope.THREAD_SCOPE, new ThreadScope()); // @1
        supper.postProcessBeanFactory(beanFactory);
      }
    };
    // 读取配置文件位置
    context.setConfigLocation(beanXml);
    // 启动容器
    context.refresh();
    // 使用容器获取bean
    for (int i = 0; i < 2; i++) { // @2
      new Thread(() -> {
        System.out.println(Thread.currentThread() + "," + context.getBean("threadBean"));
        System.out.println(Thread.currentThread() + "," + context.getBean("threadBean"));
      }).start();
      TimeUnit.SECONDS.sleep(1); 
    }
  }
}
```
## 总结
1. spring容器自带的有两种作用域，分别是singleton和prototyp,还有三种分别是 spring web 容器环境中才支持的request,session, application
2. singleton是spring容器默认的作用域，一个spring容器中同名的bean实例只有一个，多次获取得到的是同一个bean;单线程的bean需要考虑线程安全问题。
3. prototype是多例的，每次从容器中获取同名的bean，都会重新创建一个；多例bean使用的时候需要考虑创建bean对性能的影响。
4. 一个应用中可以有多个spring容器
5. 自定义scope 3个步骤，实现Scope接口，将实现类注册到spring容器，使用自定义的sope

# 依赖注入之手动注入
## 依赖对象的初始化方式
### 1. 通过构造器设置依赖对象
### 2. 通过set方法设置依赖对象
## spring依赖注入
&emsp;&emsp;spring中依赖注入主要分为手动注入和自动注入，手动注入需要我们明确配置需要注入的对象，spring也是通过两种方式注入依赖的：构造函数的方式和set属性的方式。
### 1.spring通过构造器注入
构造器的参数就是被依赖的对象，构造器注入又分为三种注入方式：  
* 根据构造器参数索引注入
* 根据构造器参数类型注入
* 根据构造器参数名称注入
#### 1.根据构造器参数索引注入
##### 用法
```xml
<bean id="" class="">
  <constructor-arg index="0" value="上海"></constructor-arg>
  <constructor-arg index="1" value="沈阳"></constructor-arg>
  <constructor-arg index="2" value="武汉"></constructor-arg>
</bean>
```
```text
constructor-arg 用户指定构造器的参数
index:构造器参数的位置
value:构造器参数的值，value只能用来给简单的参数设置值，value对应的属性类型只能为byte,int,long,float,double,boolean,Byte,Long,Float,Double,枚举,spring容器内部注入的时候会将value的值转换为对应的类型。
```
##### 优缺点
参数位置的注入对参数顺序有很强的依赖性，若构造函数的参数位置被人调整过，会导致注入出错。  
不过通常情况下，不建议去代码中修改构造函数，如果需要新增参数的，可以新增一个构造函数来实现，这算是一个扩展，不会影响目前已有的功能。
#### 2.根据构造器参数类型注入
```xml
<bean id="" class="">
  <constructor-arg type="参数类型" value="参数值"></constructor-arg>
  <constructor-arg type="参数类型" value="参数值"></constructor-arg>
  <constructor-arg type="参数类型" value="参数值"></constructor-arg>
</bean>
```
```text
constructor-arg:用户指定构造器的参数
type:构造器参数的完整类型，如：java.lang.String,int,double
value:构造器参数的值，value只能用来给简单的类型设置 值
```
#### 优缺点
&emsp;&emsp;实际上按照参数位置或者按照参数的类型注入，都有一个问题，很难通过bean的配置文件，知道这个参数是对应构造器中的哪一个属性，代码的可读性不好
### 3.根据构造器参数名称注入
```xml
<bean id="" class="">
  <constructor-arg name="参数类型" value="参数值"></constructor-arg>
  <constructor-arg name="参数类型" value="参数值"></constructor-arg>
  <constructor-arg name="参数类型" value="参数值"></constructor-arg>
</bean>
```
```text
constructor-arg:用户指定构造器的参数
name:构造参数名称
value:构造器参数的值，value只能用来给简单的类型设置 值
```
#### 关于方法参数名称问题
&emsp;&emsp;Java通过反射的方式可以获取到方法的参数名称，不过源码中的参数经过编译之后会变成.class对象，通常情况下源码变成class文件之后，参数的真实名称会丢失，参数的名称会变成arg0,arg1,arg2....这样的，和实际参数名称不一样了，如果需要将源码中的参数名称保留在编译之后的class文件中，需要用到命令：javac -parameters java源码，但是我们难以保证编译代码的时候，操作人员一定会带上-parameters参数，所以方法的参数可能在class文件中会丢失，导致反射获取到的参数名称和实际参数名称不符。  
&emsp;&emsp;参数名称可能不稳定的问题，spring提供了解决方案，通过ConstructorProperties注解来定义参数的名称，将这个注解加载构造方法上面。如下：
```java
@ConstructorProperties({"第一个参数名称", "第二个参数的名称",..."第n个参数的名称"})
public 类名(String p1, String p2...,参数n) {}
```
### setter注入
&emsp;&emsp;通常情况下我们的类都是标准的javabean,javabean类特点：  
* 属性都是private访问级别的;
* 属性通常情况下通过一组setter(修改器)和getter(访问器)方法来访问;
* setter方法，以set开头后跟首字母大写的属性名，如：setUserName,简单属性一般只有一个方法参数，方法返回值一般为void;
* getter方法，以get开头后跟首字母大写的属性名，如：getUserName,对于Boolean类型一般以is开头，后跟首字母大写的属性名，如isOk;  
spring对符合Javabean特点的类，提供了setter方式的注入，会调用对应属性setter方法将被依赖的对象注入进去;
#### 用法
```xml
<bean id="" class="">
  <property name="属性名称" value="属性值"></property>
  <property name="属性名称" value="属性值"></property>
  <property name="属性名称" value="属性值"></property>
  <property name="属性名称" value="属性值"></property>
</bean>
```
```text
property: 用于对属性的值进行配置，可以有多个
name: 属性的名称
value: 属性的值
```
#### 优缺点
&emsp;&emsp;setter注入相对于构造函数注入要灵活一些，构造函数需要指定对应构造函数中所有参数的值，而setter注入的方式没有这种限制，不需要对所有属性都要进行注入，可以按需进行注入。  
&emsp;&emsp;上面介绍的都是注入普通类型的对象，都是通过value 属性来设置需要注入的对象的值的，value属性是string类型的，spring容器内部自动会将value的值转换为对象的实际类型。  
**如果我们依赖的对象是容器中的其它bean对象的时候，需要用到下面的方式进行注入。**
### 注入容器中的bean
#### 注入容器中的bean有两种写法：
* ref属性方式
* 内置bean方式
#### ref属性方式
* 构造器方式，将value替换为ref
```xml
<constructor-arg ref="需要注入的bean的名称"/>
```
* setter方式，将value替换为ref
```xml
<property name="属性名称" ref="需要注入的bean名称"/>
```
#### 内置bean方式
* 构造器方式
```xml
<constructor-arg>
  <bean class=""/>
</constructor-arg> 
```
* setter方式
```xml
<property name="属性名称">
  <bean class=""
</property>
```