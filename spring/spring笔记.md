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
