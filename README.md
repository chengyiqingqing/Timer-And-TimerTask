# Android 之 Timer的api翻译，关于Timer的那点儿事儿。


>下文便是Timer的前世今生 —— 如写的不好，欢迎提意见，感谢

某一控件的出现，不以它的应用场景相对应的，都是耍流氓~

本文的顺序是先根据原android文档说其`特点`：然后是`应用场景`；之后是`核心api`；再之后是`与之对应的小demo`帮助你去理解。

# 特点及说明：
`A facility for threads to schedule tasks for future execution in a background thread. Tasks may be scheduled for one-time execution,
 or for repeated execution at regular intervals.`
 - 它是线程的一种工具：可以在后台线程中调度任务，以便将来执行。任务可以被调度为一次性执行，也可以定期执行重复执行。
 
 `Corresponding to each Timer object is a single background thread that is used to execute all of the timer's tasks, sequentially. 
 Timer tasks should complete quickly. If a timer task takes excessive time to complete, it "hogs" the timer's task execution thread. This can, in turn, 
 delay the execution of subsequent tasks, which may "bunch up" and execute in rapid succession when (and if) the offending task finally completes.`

- 对应于每个计时器对象是一个单独的后台线程，该线程被用来执行所有定时器的任务。定时器任务应该很快完成。
如果一个定时器任务需要花费大量的时间来完成，它会“占用”定时器的任务执行线程。这反过来又会延迟执行后续任务，
这些任务可能会“堆起来”，并在最终完成任务时快速地执行。

Timer是一个普通的类，父类为Object。 其中有几个重要的方法；而TimerTask则是一个抽象类，其中有一个抽象方法run()，类似线程中的run()方法，
我们使用Timer创建一个他的对象，然后使用这对象的schedule方法来完成这种间隔的操作。

# 应用场景：
在开发中我们有时会有这样的需求，即在固定的每隔一段时间执行某一个任务。
比如UI上的控件需要随着时间改变，我们可以使用Java为我们提供的计时器的工具类，即Timer和TimerTask。

** Timer类的api摘要与用法介绍**

**构造方法构建Timer对象**
-Timer()	该Timer对象运行在Ui线程中，可以更新Ui。
-Timer(boolean isDaemon)   isDaemon为true时 ，Timer对象运行在子线程中。
-Timer(String name)		   name则为指定运行的线程名。
-Timer(String name, boolean isDaemon) 这个就不说了，综合2,3

**核心公共方法**
-cancel()	    将这个计时器删除，丢弃任何当前计划的任务
-schedule(TimerTask task, long delay, long period)    在指定的延迟之后，将指定的任务调度为重复的固定延迟执行。
-schedule(TimerTask task, Date time)		   在指定的时间安排指定的任务执行
-schedule(TimerTask task, Date firstTime, long period)   将指定的任务调度为重复的固定延迟执行，从指定的时间开始。
-schedule(TimerTask task, long delay)     在指定的延迟之后安排指定的任务执行


