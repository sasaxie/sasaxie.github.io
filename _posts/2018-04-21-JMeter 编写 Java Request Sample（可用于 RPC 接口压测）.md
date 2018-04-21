---
title: JMeter 编写 Java Request Sample（可用于 RPC 接口压测）
date: 2018-04-21 16:43:18
categories:
- 测试
tags:
- 测试
- 压测
- JMeter
- RPC
---
# 1. Java 代码实例

## 1.1 依赖

Gradle

```shell
compile group: 'org.apache.jmeter', name: 'ApacheJMeter_java', version: '4.0'
```

Maven

```shell
<dependency>
    <groupId>org.apache.jmeter</groupId>
    <artifactId>ApacheJMeter_java</artifactId>
    <version>4.0</version>
</dependency>
```

## 1.2 代码

```java
package top.xiexiaodong.example;

import org.apache.jmeter.config.Arguments;
import org.apache.jmeter.protocol.java.sampler.AbstractJavaSamplerClient;
import org.apache.jmeter.protocol.java.sampler.JavaSamplerContext;
import org.apache.jmeter.samplers.SampleResult;

public class NormalJavaRequestSample extends AbstractJavaSamplerClient {

  private SampleResult result;
  private int num1;
  private int num2;

  /**
   * 初始化方法，用于初始化性能测试时的每个线程，每个线程测试前执行一次。
   */
  @Override
  public void setupTest(JavaSamplerContext context) {
    result = new SampleResult();

    num1 = context.getIntParameter("num1");
    num2 = context.getIntParameter("num2");

    // 可以初始化 RPC Client
  }

  /**
   * 测试结束时调用，可释放资源等。
   */
  @Override
  public void teardownTest(JavaSamplerContext context) {
    System.out.println("NormalJavaRequestSample.teardownTest");
  }

  /**
   * 主要用于设置传入的参数和默认值，可在 Jmeter 界面显示。
   */
  @Override
  public Arguments getDefaultParameters() {
    Arguments arguments = new Arguments();

    arguments.addArgument("num1", "1");
    arguments.addArgument("num2", "2");

    return arguments;
  }

  /**
   * 性能测试运行体。
   */
  @Override
  public SampleResult runTest(JavaSamplerContext context) {
    result.sampleStart(); // Jmeter 开始计时

    boolean success = add(num1, num2);

    result.setSuccessful(success); // 是否成功
    result.sampleEnd(); // Jmeter 结束计时

    return result;
  }

  private boolean add(int a, int b) {
    System.out.println(a+b);

    return true;
  }

  /**
   * Jmeter 不会调用 main 方法，这里用于生成 Jar。
   * @param args
   */
  public static void main(String[] args) {
    Arguments arguments = new Arguments();
    arguments.addArgument("num1", "1");
    arguments.addArgument("num2", "2");

    JavaSamplerContext context = new JavaSamplerContext(arguments);
    NormalJavaRequestSample sample = new NormalJavaRequestSample();
    sample.setupTest(context);
    sample.runTest(context);
    sample.teardownTest(context);
  }
}
```

## 1.3 JMeter

安装完 JMeter，将上面代码生成可执行 Jar 包，放到 `JMeter/lib/ext/` 目录，然后打开 JMeter。

### 1.3.1 创建 Thread Group

右键 `Test Plan` -> `Add` -> `Thread(Users)` -> `Thread Group`

### 1.3.2 创建 Sample

右键新创建的 `Thread Group` -> `Add` -> `Sampler` -> `Java Request`，Classname 选择相应的类名，这里是 `top.xiexiaodong.example.NormalJavaRequestSample`

会出现预设的参数列表：

![1](/source/image/2018-04-21-1/1.png)