# 简介

[官方教程](http://www.quartz-scheduler.org/documentation/2.4.0-SNAPSHOT/tutorials/)

开源作业调度库。提供在指定时间触发任务执行以及定期执行任务功能。

功能：
- 多种运行环境：应用程序、服务
- 灵活配置执行周期
- 监控任务执行、完成情况
- 集群特性
- 持久化

关键概念：

- Schedule：调度器，调度执行任务
- Job：任务，一个任务可以绑定多个触发器
- Trigger：触发器，用于触发（fire）任务的执行。一个触发器可以绑定多个任务
- JobBuilder
- TriggerBuilder
- DateBuilder：方便构建指定时间的 `java.util.Date`

# 安装

```
<dependency>
    <groupId>org.quartz-scheduler</groupId>
    <artifactId>quartz</artifactId>
    <version>2.3.2</version>
</dependency>
```

# Schedule

生命周期：从 start -> shutdown，只有 start 后才能对任务进行调度执行。

```
Scheduler defaultScheduler = StdSchedulerFactory.getDefaultScheduler();  
defaultScheduler.start();

// ...

defaultScheduler.shutdown();
```

- 添加、删除、列出 Job 和 Trigger
- 对 Job 和 Trigger 进行调度

# Job 和 JobDetail

Job 的生命周期：schedule 每次执行 Job 时会创建一个新的实例（调用无参构造器），execute 方法执行完后该实例便被销毁，故 Job 是无状态的。

如何让无状态的 Job 实现有状态？Job 无状态，那么就把状态保存在 Job 外面，实例化时传进来。问题转变为了：如何给 Job 传参？

有两种传参方式：
1. 通过 JobDetail 的 JobDataMap
2. 通过与 Job 绑定的 tigger 的 JobDataMap

```java
// define the job and tie it to our DumbJob class
  JobDetail job = newJob(DumbJob.class)
      .withIdentity("myJob", "group1") // name "myJob", group "group1"
      .usingJobData("jobSays", "Hello World!")
      .usingJobData("myFloatValue", 3.141f)
      .build();

public interface Job {
    void execute(JobExecutionContext context) throws JobExecutionException;
}

public class DumbJob implements Job {

    public void execute(JobExecutionContext context) throws JobExecutionException {
      JobKey key = context.getJobDetail().getKey();

      JobDataMap dataMap = context.getJobDetail().getJobDataMap();

      String jobSays = dataMap.getString("jobSays");
      float myFloatValue = dataMap.getFloat("myFloatValue");

      System.err.println("Instance " + key + " of DumbJob says: " + jobSays + ", and val is: " + myFloatValue);
    }
}
```

如何获取通过 JobDetail 的 JobDataMap？
`context.getJobDetail().getJobDataMap()`

如何获取通过与 Job 绑定的 tigger 的 JobDataMap？
`context.getMergedJobDataMap()`

需要注意的是，getMergedJobDataMap 中既包含了 JobDetail 中的 key-val，也包含了 trigger 中的 key-val 且后者会覆盖前者相同的 key。

[有关 Job 和 trigger 更多细节](http://www.quartz-scheduler.org/documentation/2.4.0-SNAPSHOT/tutorials/tutorial-lesson-03.html)

> If you add setter methods to your job class that correspond to the names of keys in the JobDataMap (such as a setJobSays (String val) method for the data in the example above), then Quartz’s default JobFactory implementation will automatically call those setters when the job is instantiated, thus preventing the need to explicitly get the values out of the map within your execute method.
>
> Triggers can also have JobDataMaps associated with them. This can be useful in the case where you have a Job that is stored in the scheduler for regular/repeated use by multiple Triggers, yet with each independent triggering, you want to supply the Job with different data inputs.
>
> The JobDataMap that is found on the JobExecutionContext during Job execution serves as a convenience. It is a merge of the JobDataMap found on the JobDetail and the one found on the Trigger, with the values in the latter overriding any same-named values in the former.

# Trigger

trigger 用来触发任务的执行。

常用的 trigger 是：SimpleTrigger 和 CronTrigger。

SimpleTrigger 可以简单设置触发时间，触发间隔，重复次数；而 CronTrigger 可以提供类似日历（按星期，年月日等进行调度）的触发形式。

JobDataMap

# Key

Trigger 和 Job 在 Schedule 中以 (name, groupName) 作为唯一标识。
