## 什么是stream

官方解释：
> A sequence of elements supporting sequential and parallel aggregate operations.

简单来讲，stream就是JAVA8提供给我们的对于元素集合统一、快速、并行操作的一种方式。它能充分运用多核的优势，以及配合lambda表达式、链式结构对集合等进行许多有用的操作。

使用stream，我们能够对collection的元素进行过滤、映射、排序、去重等许多操作。

**中间方法和终点方法：**

它具有过滤、映射以及减少遍历数等方法，这些方法分两种：中间方法和终端方法，
“流”抽象天生就该是持续的，中间方法永远返回的是Stream，因此如果我们要获取最终结果的话，
必须使用终点操作才能收集流产生的最终结果。区分这两个方法是看他的返回值，
如果是Stream则是中间方法，否则是终点方法

大家可以把Stream当成一个高级版本的Iterator。原始版本的Iterator，用户只能一个一个的遍历元素并对其执行某些操作；高级版本的Stream，用户只要给出需要对其包含的元素执行什么操作，比如“过滤掉长度大于10的字符串”、“获取每个字符串的首字母”等，具体这些操作如何应用到每个元素上，就给Stream就好了！

下面介绍几个转换Stream的函数

- <u>*distinct():*</u>
 对于Stream中包含的元素进行去重操作（去重逻辑依赖元素的equals方法），新生成的Stream中没有重复的元素；

- <u>*filter():*</u>
对于Stream中包含的元素使用给定的过滤函数进行过滤操作，新生成的Stream只包含符合条件的元素；

- <u>*map():*</u>
对于Stream中包含的元素使用给定的转换函数进行转换操作，新生成的Stream只包含转换生成的元素。这个方法有三个对于原始类型的变种方法，分别是：mapToInt，mapToLong和mapToDouble。这三个方法也比较好理解，比如mapToInt就是把原始Stream转换成一个新的Stream，这个新生成的Stream中的元素都是int类型。之所以会有这样三个变种方法，可以免除自动装箱/拆箱的额外消耗；

- <u>*flatMap():*</u>
和map类似，不同的是其每个元素转换得到的是Stream对象，会把子Stream中的元素压缩到父集合中；

- <u>*peek():*</u>
生成一个包含原Stream的所有元素的新Stream，同时会提供一个消费函数（Consumer实例），新Stream每个元素被消费的时候都会执行给定的消费函数；

- <u>*limit():*</u>
对一个Stream进行截断操作，获取其前N个元素，如果原Stream中包含的元素个数小于N，那就获取其所有的元素；

- <u>*skip():*</u>
返回一个丢弃原Stream的前N个元素后剩下元素组成的新Stream，如果原Stream中包含的元素个数小于N，那么返回空Stream；

下面来举个例子
```
List<Integer> nums = Lists.newArrayList(1,1,null,2,3,4,null,5,6,7,8,9,10);
System.out.println(“sum is:”+nums.stream().filter(num -> num != null).
   			distinct().mapToInt(num -> num * 2).
               peek(System.out::println).skip(2).limit(4).sum());
```

简单解释一下这段代码的含义：给定一个Integer类型的List，获取其对应的Stream对象，然后进行过滤掉null，再去重，再每个元素乘以2，再每个元素被消费的时候打印自身，在跳过前两个元素，最后去前四个元素进行加和运算
