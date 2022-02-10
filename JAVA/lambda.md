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

4. flatMap 将多个stream合并为一个Stream。惰性求值,调用Stream.of的静态方法将两个list转换为Stream，再通过flatMap将两个流合并为一个。
```java
public class TestCase {
    public static void main(String[] args) {
        List<Student> students = new ArrayList<>(3);
        students.add(new Student("路飞", 22, 175));
        students.add(new Student("红发", 40, 180));
        students.add(new Student("白胡子", 50, 185));
        List<Student> studentList = Stream.of(students,
                asList(new Student("艾斯", 25, 183),
                        new Student("雷利", 48, 176)))
                .flatMap(students1 -> students1.stream()).collect(Collectors.toList());
        System.out.println(studentList);
    }
}
//输出结果
//[Student{name='路飞', age=22, stature=175, specialities=null}, 
//Student{name='红发', age=40, stature=180, specialities=null}, 
//Student{name='白胡子', age=50, stature=185, specialities=null}, 
//Student{name='艾斯', age=25, stature=183, specialities=null},
//Student{name='雷利', age=48, stature=176, specialities=null}]
```


5. max和min
	* max、min接收一个Comparator（例子中使用java8自带的静态函数，只需要传进需要比较值即可。）并且返回一个Optional对象，该对象是java8新增的类，专门为了防止null引发的空指针异常。可以使用max.isPresent()判断是否有值；可以使用max.orElse(new Student())，当值为null时就使用给定值；也可以使用max.orElseGet(() -> new Student());这需要传入一个Supplier的lambda表达式。
```java
public class TestCase {
    public static void main(String[] args) {
        List<Student> students = new ArrayList<>(3);
        students.add(new Student("路飞", 22, 175));
        students.add(new Student("红发", 40, 180));
        students.add(new Student("白胡子", 50, 185));
        Optional<Student> max = students.stream()
            .max(Comparator.comparing(stu -> stu.getAge()));
        Optional<Student> min = students.stream()
            .min(Comparator.comparing(stu -> stu.getAge()));
        //判断是否有值
        if (max.isPresent()) {
            System.out.println(max.get());
        }
        if (min.isPresent()) {
            System.out.println(min.get());
        }
    }
}
//输出结果
//Student{name='白胡子', age=50, stature=185, specialities=null}
//Student{name='路飞', age=22, stature=175, specialities=null}
```

6. count 统计功能，一般都是结合filter使用，因为先筛选出我们需要的再统计即可。及早求值
```java
public class TestCase {
    public static void main(String[] args) {
        List<Student> students = new ArrayList<>(3);
        students.add(new Student("路飞", 22, 175));
        students.add(new Student("红发", 40, 180));
        students.add(new Student("白胡子", 50, 185));
        long count = students.stream().filter(s1 -> s1.getAge() < 45).count();
        System.out.println("年龄小于45岁的人数是：" + count);
    }
}
//输出结果
//年龄小于45岁的人数是：2
```

7. reduce
	* reduce操作可以实现从一组值中生成一个值。在上述例子中用到的count(),max(),min()方法因为常用而被纳入标准库中。事实上，这些方法都是reduce操作。及早求值
	 ```java
	 public class TestCase {
	     public static void main(String[] args) {
	         Integer reduce = Stream.of(1, 2, 3, 4).reduce(0, (acc, x) -> acc+ x);
	         System.out.println(reduce);
	     }
	 }
	 //输出结果
	 //10
	```
	* reduce接收了一个初始值为0的累加器，依次取出值与累加器相加，最后累加器的值就是最终的结果。

## 高级集合类及收集器
1. 转换成值
2. 转换成块
3. 数据分组
4. 字符串拼接