## 函数式接口
1. 什么是函数式接口
* 函数式接口是只有一个抽象方法的接口，用作lambda表达式的类型。使用@FunctionalInterface注解修饰类，编译器会检测该类是否只有一个抽象方法或接口，否则，会报错。可以有多个默认方法，静态方法。
* Java8自带的常用函数式接口。

 |函数接口 |抽象方法 |功能 |参数 |返回类型 |示例 |
 | --      | --     | --  | --  | --      | --  |
 |Predicate|test(T t)|判断真假|T|boolean|9龙的身高大于185cm吗？|
 |Consumer|accept(T t)|消费消息|T|void|输出一个值|
 |Function|R apply(T t)|将T映射为R（转换功能）|T|R|获得student对象的名字|
 |Supplier|T get()|生产消息|None|T|工厂方法|
 |UnaryOperator|T apply(T t)|一元操作|T|T|逻辑非（!）|
 |BinaryOperator|apply(T t, U u)|二元操作|(T，T)|(T)|求两个数的乘积（*）|
 
 ## 常用的流
1. collect(Collectors.toList()) 将流转换为list。还有toSet(),toMap等。及早求值
```java
public class TestCase {
    public static void main(String[] args) {
        List<Student> studentList = Stream.of(new Student("路飞", 22, 175),
                new Student("红发", 40, 180),
                new Student("白胡子", 50, 185)).collect(Collectors.toList());
        System.out.println(studentList);
    }
}
//输出结果
//[Student{name='路飞', age=22, stature=175, specialities=null}, 
//Student{name='红发', age=40, stature=180, specialities=null}, 
//Student{name='白胡子', age=50, stature=185, specialities=null}]
```

2. filter 过滤筛选的作用，内部就是Predicate接口，惰性求值
```java
public class TestCase {
    public static void main(String[] args) {
        List<Student> students = new ArrayList<>(3);
        students.add(new Student("路飞", 22, 175));
        students.add(new Student("红发", 40, 180));
        students.add(new Student("白胡子", 50, 185));
        List<Student> list = students.stream()
            .filter(stu -> stu.getStature() < 180)
            .collect(Collectors.toList());
        System.out.println(list);
    }
}
//输出结果
//[Student{name='路飞', age=22, stature=175, specialities=null}]
```

3. map 转换功能，内部就是Function接口。惰性求值
```java
public class TestCase {
    public static void main(String[] args) {
        List<Student> students = new ArrayList<>(3);
        students.add(new Student("路飞", 22, 175));
        students.add(new Student("红发", 40, 180));
        students.add(new Student("白胡子", 50, 185));
        List<String> names = students.stream().map(student -> student.getName())
                .collect(Collectors.toList());
        System.out.println(names);
    }
}
//输出结果
//[路飞, 红发, 白胡子]
// 例子中将student对象转换为String对象，获取student的名字。
```

4. flatMap 将多个stream合并为一个Stream。惰性求值