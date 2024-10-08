---
title: Java编程规约
date: 2024-10-08 14:01:00
tags:
    - Java
    - Java开发手册
    - 规范
    - 习惯
categories:
    - Java
    - 开发手册
---
## 日期时间

1. 日期格式化时，传入pattern中表示年份统一用小写的yyyy，大写的Y会在最后一周跨年时返回下一周。

2. 使用枚举值来指代月份时，如果使用数字，注意Date，Calendar等日期相关的月份month取值范围从0到11之间。(0 for January)

## 集合处理

1. 判断所有集合内部的元素是否为空，使用isEmpty()方法，不用size()==0。  
前者的时间复杂度为O(1)，可读性更好。

2. ArrayList的sublist结果不可强转成ArrayList，否则会抛出ClassCastException  

sublist只是ArrayList的一个视图，一个ArrayList的内部类SubList，而不是ArrayList本身。  
可以用new 一个ArrayList，以这个sublist做参数的方法把子集合单独提出来进行操作。

3. 在subList场景中，**高度注意**对父集合元素的增加或删除，均会导致子列表的遍历、增加、删除产生ConcurrentModification异常。

4. 使用集合转数组的方法，必须使用集合的toArray(T[] array)，传入的时类型完全一致，长度为0的空数组。

        例：  
        List<String> list = new ArrayList<>(2);
        String[] array = list.toArray(new String[0]);

5. 使用工具类Array.asList()把数组转换为集合时，不能使用其修改集合相关的方法add / remove / clear，会抛出UnsupportedOperationException异常。

        String[] str = new String[]{"sun","yu","sys"};
        List list = Arrays.asList(str);

这里str改变时list也会跟着改变，反之亦然。

6. 泛型通配符<? extend T>来接收返回的数据，此写法的泛型集合不能使用add方法  
而<? super T>不能使用get方法

extend：上界通配符，可以接受T和T的子类，读取数据  
super：下界通配符，可以接受T和T的父类，写入数据

        例：
        public void printNumbers(List<? extend Number> list) {
            for (Number numbeeeeer : list) {
                System.out.println(number);
            }
        }
        public void addIntegers(List<? super Integer> list) {
            list.add(0);
            list.add(1);
        }

7. 不要在foreach循环里进行元素的remove/add操作  
remove元素使用iterator，如果并发操作需要对iterator对象加锁  

        例：
        List<String> list = new ArrayList<>();
        Iterator<String> iterator = list.itertor();
        while(itertor.hasNext()) {
            String item = itertor.next();
            itertor.remove;//安全删除
        }

foreach循环本质上是依赖于集合的迭代器，修改集合会破坏迭代器的状态。