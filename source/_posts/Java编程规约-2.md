---
title: Java编程规约
date: 2024-10-10 16:23:57
tags:
    - Java
    - Java开发手册
    - 规范
    - 习惯
categories:
    - Java
    - 开发手册
---
## 并发处理

1. 线程资源必须通过线程池提供，不允许在应用中显式地创建线程。  
线程池能减少线程在创建和销毁上消耗的时间以及系统资源。

2. 线程池不允许使用`Executors`去创建，而是通过`ThreadPoolExecutor`的方式。规避资源耗尽的风险。

3. `SimpleDatreFormat`是线程不安全的类，不要定义成static变量，如果要求static必须加锁，或者使用DateUtils工具类。

4. 必须回收自定义的`TreadLocal`变量，尽量用try-finally块进行回收。

5. 高并发时，同步调用应取考量锁的性能损耗。  
能用无锁数据结构就不要用锁。  
能锁区块就不要锁整个方法体。  
能能对象锁就不要用类锁。  
（避免在锁代码中调用RPC方法）

6. 对多个资源、数据库表、对象同时加锁时，需要保持一致的加锁顺序，否则可能导致死锁。（每个线程加锁顺序应一致）

7. 在使用阻塞等待锁的方法中，必须在try代码块之外，并且在加锁与try之间没有任何可能抛出异常的方法调用，避免无法解锁导致其他线程无法成功获取锁。

        例:
        Lock lock = new XxxLock();
        lock.lock();
        try {
            doSomething():
        } finally {
            lock.unlock();
        }

8. 避免Random实例被多线程使用，该函数是线程安全的，但是因竞争同一seed会使性能能下降。

## 控制语句

1. 在if/else/for/while/do语句中必须使用大括号。

2. 在高并发场景中避免使用`==`作为中断和推出的条件。  
如果并发没有控制好，容易发生等值被击穿的情况，建议使用大于或者小于的区间作为判断条件。

3. 表达分支异常时，少用if-else方式，请勿超过3层。

        可以这样代替：
        if(man.isHigh()){
            System.out.println("真高")；
        }
        if(man.isHandsome()){
            System.out.println("真帅");
        }
        if(man.isRich()){
            System.out.println("真有钱");
        }

4. 不要再条件判断中执行复杂的语句，将复杂逻辑判断的结果赋值给一个有意义的布尔变量名，提高可读性。