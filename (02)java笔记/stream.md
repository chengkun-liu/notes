# 一、lambda

## 1. 四大函数式接口

## 2. 方法、构造器和数组引用

# 二、stream流

## 1. 创建流

1. 通过collection系列集合提供的stream()和parallelStream()
2. 通过Arrays的静态方法stream()获取数组流
3. 通过Stream的静态方法of()
4. 创建Stream的创建无线无限流
   1. 迭代iterate()
   2. 生成generate()

## 2. 中间操作

不会执行任务操作，只有执行终止操作了，才一次性执行

1. 帅选与切片
   1. filter：接受lambda表达式，Predicate函数
   2. limit：截断流，使其元素不超过给定的数量
   3. skip：跳过前n个元素，如果流中元素不足n个，则返回一个空流，与limit互补
   4. distinct：去重，通过流中元素的hashCode和equals去重，如果是自定义元素，要重写这两个方法
2. Map
   1. map
   2. flatmap：双重for循环
3. 排序
   1. sorted：自然排序（Comparable==》）
   2. sorted(Comparator com)：定制排序

## 3. 终止操作

1. 查找与匹配
   
   1. allMatch：检查给定的元素是否与流中的所有元素匹配，匹配-true，不匹配-false
   2. anyMatch：检查给定的元素是否与流中的任意元素匹配，匹配-true，不匹配-false
   3. noneMatch：检查给定的元素是否与流中的所有元素不匹配，匹配-false，不匹配-true
   4. findFirst：返回第一个元素
   5. findAny：返回当前流中任意元素
   6. count：返回流中元素的个数
   7. max：返回流中最大值
   8. min：返回流中最小值
   
2. 归约

   * 可以将流中的元素反复结合起来，按给定的逻辑运算得到一个值。通常和Map合用

   1. reduce(T identity-起始值，BinaryOperator-运算表达式)
   2. reduce(BinaryOperator)

3. 收集

   1. Collect(Collectors.toList())
   2. Collect(Collectors.toSet())
   3. Collect(Collectors.toCollection(HashSet::new))
   4. Collect(Collectors.counting())
   5. Collect(Collectors.averagingDouble(Emplee::getSalary))
   6. Collect(Collectors.summingDouble(Emplee::getSalary))
   7. Collect(Collectors.maxBy())
   8. Collect(Collectors.minBy())
   9. Collect(Collectors.groupBy())：单级分组
   10. Collect(Collectors.groupBy(Emplee::getStatus),Collectors.groupBy())：多级分组
   11. Collect(Collectors.partitioningBy())：分区，满足条件一个区，不满足条件一个区
   12. Collect(Collectors.summarizingDouble(Emplee::getSalary))：返回一个DoubleSummaryStatistics，封装了最大值，最小值等
   13. Collect(Collectors.joining("连接字符串之间使用什么区分开"，"第一个字符串之前是什么"，"最后一个字符串后面是什么"))

```Java
spring断言工具类使用
Assert.notNull(Object object, "object is required")    -    对象非空 
Assert.isTrue(Object object, "object must be true")   -    对象必须为true   
Assert.notEmpty(Collection collection, "collection must not be empty")    -    集合非空  
Assert.hasLength(String text, "text must be specified")   -    字符不为null且字符长度不为0   
Assert.hasText(String text, "text must not be empty")    -     text 不为null且必须至少包含一个非空格的字符  
Assert.isInstanceOf(Class clazz, Object obj, "clazz must be of type [clazz]")    -    obj必须能被正确造型成为clazz 指定的类
```

```Java
select 'GRANT INSERT,SELECT,DELETE,UPDATE ON TBOS.TB_CHECKNODE_PARAM to KS_APPROVE;'  from user_tables

-- 用户权限授权
GRANT INSERT,SELECT,DELETE,UPDATE ON TBOS.TB_CHECKNODE_PARAM to KS_APPROVE;

-- sequence授权
grant SELECT  on TBOS.TB_CHECKNODE_PARAM_SEQ to KS_APPROVE
```

```
字节流和字符流如果是空的，在读取时会发生阻塞问题
available，可以查看有多少可用字节，如果是本地读取不会有问题，如果是网络读取会有出现数据差异，原因是从网络发送过来的数据，有可能是分批发送，获取有延迟，
```



SpringBoot中如何控制bean的加载顺序

一、@AutoConfigureOrder只能改变外部依赖的@ConfigureOrder的顺序

1. 内部：能被你工程内部scan到的包，都是内部Configuration
2. 外部：spring引入的外部Configuration，都是通过spring特有的spi文件：spring.factories
3. 理解：@AutoConfigureOrder能改变spring.factories中的@Configuration的顺序

二、@DependsOn

三、参数注入，在@Bean中的方法上，如果传入参数，Springboot会自动在容器里面寻找这个引用，并先初始化这个类的实例。利用此特性可以控制bean的加载顺序

四、利用bean的声明周期的扩展点



Maven依赖冲突

ommited(删除) for conflict with，可能不会影响程序运行，但是极端会出现类找不到异常

单个排除：exclusion排除即可，最外层的依赖进行排除

批量排除：Maven Helper

























