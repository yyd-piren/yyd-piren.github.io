---
title: Java编程规约
date: 2024-10-06 22:46:19
tags:
    - Java
    - Java开发手册
    - 规范
    - 习惯
categories:
    - Java
    - 开发手册
---
## 命名风格

1. 类名使用UpperCamelCase风格

2. 方法名、参数名、成员变量和局部变量都统一使用lowerCamelCase风格

3. 如果模块、接口、类和方法使用了设计模式，再命名时要体现出具体模式。  
        例：public class OrderFactory;
            public class LoginProxy;
                public class ResourceObserver;

4. 接口类中的方法和属性不要加任何修饰符号（public也不要加），保持代码的简洁性，并加上有效的javadoc注释。尽量不要在接口里定义常量，如果一定要定义，最好确定该常量与接口的方法相关，并且时整个应用的基础常量。

## 常量定义

1. 不允许任何魔法值（未经预先定义的常量）直接出现在代码中。  
`反例：String key = "Id#taobao" + tradeId;`

2. 不要使用一个常量类维护所有常量，要按常量功能进行归类，分开维护。

## 代码格式

1. if/ for/ while/ switch/ do等保留字与左右括号之间必须加空格。  
`if (a == b) {}`

2. 任何二目、三目运算符的左右两边都需要加一个空格（包括赋值运算符 =、逻辑运算符 &&、加减乘除符号等）。

3. 采用4个空格缩进，如果要用tab必须设置为一个tab四个空格

4. IDE的text file encoding设置为UTF-8；IDE中文件的换行符使用Unix格式，不要使用Windows格式。

## OOP规约

1. 避免通过一个类的对象引用访问此类的静态变量或静态方法，无谓增加成本，直接用类名来访问。

2. 所有的覆写方法必须加@Override注解。

3. Object的equals方法容易抛空指针异常，应使用常量或确定有值的对象来调用equals。

4. 所有整型包装类对象之间值的比较，全部使用equals方法比较。

5. 浮点数之间的等值比较，基本数据类型不能使用==来比较，包装数据类型不能用equals来比较。  

        正例：使用误差或者BigDecimal  
        (1) float a = 1.0F - 0.9F;
        float b = 0.9F - 0.8F;
        float diff = 1e-6F;
        if (Math.abs(a - b) < diff) {
            System.out.println("true");
        }  
        (2)BigDecimal a = new BigDecimal("1.0");  
        BigDecimal b = new BigDecimal("0.9");  
        BigDecimal c = new BigDecimal("0.8");  
        BigDecimal x = a.subtract(b);  
        BigDecimal y = b.subtract(c);
        if(x.compareTo(y) == 0) {
            System.out.println("true");
        }

6. BigDecimal的等值比较应使用compareTo，而不是equals

7. 禁止使用BigDecimal(double)这种方法转化  

        正例：BigDecimal a = new BigDecimal("0.1");//用字符串
        BigDecimal b = BigDecimal.valueof(0.1);//valueof

8. 类内方法定义顺序：公有/保护 > 私有 > getter/setter

9. 在getter和setter中不能增加业务逻辑增加问题排查的难度

10. 循环体内，字符串的连接方式使用StringBuilder的append方法进行扩展，避免内存浪费


学累了写一写这个也不错😊