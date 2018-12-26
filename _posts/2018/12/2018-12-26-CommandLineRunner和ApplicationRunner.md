---
layout: post
title:  "CommandLineRunner接口和ApplicationRunner接口"
categories: springboot
tags: CommandLineRunner ApplicationRunner
author: cjie
---
* content
{:toc}

![Runner](https://wx2.sinaimg.cn/mw1024/005xB1vLly1fyit898mbfj30k00b9ab4.jpg)
## 前言
我们有时候可能需要写个循环任务或者定时任务，想用spring作为容器，这时候用springboot来开发会很方便。
maven或gradle构建一个小项目并且打成jar包扔服务器上nohup java -jar或者写个shell脚本用crontab定时任务来执行就行，很轻量。
SpringBoot给我们提供了两个接口来帮助我们实现这种需求。这两个接口分别为CommandLineRunner和ApplicationRunner。他们的执行时机为容器启动完成的时候。
### 1：CommandLineRunner接口和ApplicationRunner接口的共同之处与不同之处
这两个接口中有一个run方法，我们只需要实现这个方法即可。
这两个接口的不同之处在于：ApplicationRunner中run方法的参数为ApplicationArguments，而CommandLineRunner接口中run方法的参数为String数组。
下面我写两个简单的例子，来看一下这两个接口的实现。
### 2：代码示例
CommandLineRunner代码如下：
```java
package com.bianla.runner;

import lombok.extern.slf4j.Slf4j;
import org.springframework.boot.CommandLineRunner;
import org.springframework.core.annotation.Order;
import org.springframework.stereotype.Component;

/**
 * @Author cjie
 * @Date 2018/12/26 0026.
 */
@Component
@Order(2)
@Slf4j
public class TestCommandLineRunner implements CommandLineRunner {
    @Override
    public void run(String... strings) throws Exception {
        log.info("====================================================CommandLineRunner===================================");
        log.info("stirng.length=" + strings.length);
        int num = 1;
        for (String arg : strings) {
            log.info("第{}个参数，arg={}", num, arg);
            num++;
        }
    }
}

```
ApplicationRunner代码如下：
```java
package com.bianla.runner;

import com.alibaba.fastjson.JSONObject;
import lombok.extern.slf4j.Slf4j;
import org.springframework.boot.ApplicationArguments;
import org.springframework.boot.ApplicationRunner;
import org.springframework.core.annotation.Order;
import org.springframework.stereotype.Component;

/**
 * @Author cjie
 * @Date 2018/12/26 0026.
 */
@Component
@Order(1)
@Slf4j
public class TestApplicationRunner implements ApplicationRunner {
    @Override
    public void run(ApplicationArguments applicationArguments) throws Exception {
        log.info("====================================================ApplicationRunner===================================");
        log.info("applicationArguments={}", applicationArguments);
        log.info("applicationArguments.containsOption(\"cjie\")={}", applicationArguments.containsOption("cjie"));
        log.info("applicationArguments.getNonOptionArgs()={}", applicationArguments.getNonOptionArgs());
        log.info("applicationArguments.getOptionNames()={}", applicationArguments.getOptionNames());
        String[] sourceArgs = applicationArguments.getSourceArgs();
        log.info("sourceArgs.length={}", sourceArgs.length);
        log.info("applicationArguments.getOptionValues(\"hehe\")={}",applicationArguments.getOptionValues("hehe"));
        int num = 1;
        for (String arg : sourceArgs) {
            log.info("第{}个参数，arg={}", num, arg);
            num++;
        }
        log.info(JSONObject.toJSONString(applicationArguments));
    }
}
```
启动参数如下:
```text
java -jar task-1.0.jar hehe lala cjie -t=12 -spring.profiles.active=test --Dfhe=sda --hehe=lala,xixi,zizi
```
启动后日志如下：
```text
2018-12-26 15:30:27.063  INFO 9292 --- [           main] com.bianla.runner.TestApplicationRunner  : applicationArguments=org.springframework.boot.DefaultApplicationArguments@2e377400
2018-12-26 15:30:27.067  INFO 9292 --- [           main] com.bianla.runner.TestApplicationRunner  : applicationArguments.containsOption("cjie")={false
2018-12-26 15:30:27.074  INFO 9292 --- [           main] com.bianla.runner.TestApplicationRunner  : applicationArguments.getNonOptionArgs()=[hehe, lala, cjie, -t=12, -spring.profiles.active=test]
2018-12-26 15:30:27.085  INFO 9292 --- [           main] com.bianla.runner.TestApplicationRunner  : applicationArguments.getOptionNames()=[hehe, Dfhe]
2018-12-26 15:30:27.090  INFO 9292 --- [           main] com.bianla.runner.TestApplicationRunner  : sourceArgs.length=7
2018-12-26 15:30:27.100  INFO 9292 --- [           main] com.bianla.runner.TestApplicationRunner  : applicationArguments.getOptionValues("hehe")=[lala,xixi,zizi]
2018-12-26 15:30:27.106  INFO 9292 --- [           main] com.bianla.runner.TestApplicationRunner  : 第1个参数，arg=hehe
2018-12-26 15:30:27.116  INFO 9292 --- [           main] com.bianla.runner.TestApplicationRunner  : 第2个参数，arg=lala
2018-12-26 15:30:27.124  INFO 9292 --- [           main] com.bianla.runner.TestApplicationRunner  : 第3个参数，arg=cjie
2018-12-26 15:30:27.126  INFO 9292 --- [           main] com.bianla.runner.TestApplicationRunner  : 第4个参数，arg=-t=12
2018-12-26 15:30:27.133  INFO 9292 --- [           main] com.bianla.runner.TestApplicationRunner  : 第5个参数，arg=-spring.profiles.active=test
2018-12-26 15:30:27.140  INFO 9292 --- [           main] com.bianla.runner.TestApplicationRunner  : 第6个参数，arg=--Dfhe=sda
2018-12-26 15:30:27.149  INFO 9292 --- [           main] com.bianla.runner.TestApplicationRunner  : 第7个参数，arg=--hehe=lala,xixi,zizi
2018-12-26 15:30:27.239  INFO 9292 --- [           main] com.bianla.runner.TestApplicationRunner  : {"nonOptionArgs":["hehe","lala","cjie","-t=12","-spring.profiles.active=test"],"optionNames":["hehe"
,"Dfhe"],"sourceArgs":["hehe","lala","cjie","-t=12","-spring.profiles.active=test","--Dfhe=sda","--hehe=lala,xixi,zizi"]}
2018-12-26 15:30:27.239  INFO 9292 --- [           main] com.bianla.runner.TestCommandLineRunner  : ====================================================CommandLineRunner===============================
====
2018-12-26 15:30:27.240  INFO 9292 --- [           main] com.bianla.runner.TestCommandLineRunner  : stirng.length=7
2018-12-26 15:30:27.248  INFO 9292 --- [           main] com.bianla.runner.TestCommandLineRunner  : 第1个参数，arg=hehe
2018-12-26 15:30:27.255  INFO 9292 --- [           main] com.bianla.runner.TestCommandLineRunner  : 第2个参数，arg=lala
2018-12-26 15:30:27.264  INFO 9292 --- [           main] com.bianla.runner.TestCommandLineRunner  : 第3个参数，arg=cjie
2018-12-26 15:30:27.265  INFO 9292 --- [           main] com.bianla.runner.TestCommandLineRunner  : 第4个参数，arg=-t=12
2018-12-26 15:30:27.271  INFO 9292 --- [           main] com.bianla.runner.TestCommandLineRunner  : 第5个参数，arg=-spring.profiles.active=test
2018-12-26 15:30:27.280  INFO 9292 --- [           main] com.bianla.runner.TestCommandLineRunner  : 第6个参数，arg=--Dfhe=sda
2018-12-26 15:30:27.286  INFO 9292 --- [           main] com.bianla.runner.TestCommandLineRunner  : 第7个参数，arg=--hehe=lala,xixi,zizi
```
由上面的demo可以看出，参数以--开头的就是option,是--key=value1,value2,value3....的形式传递
### 3： @Order注解
如果有多个实现类，而你需要他们按一定顺序执行的话，可以在实现类上加上@Order注解。@Order(value=整数值)。SpringBoot会按照@Order中的value值从小到大依次执行。
 