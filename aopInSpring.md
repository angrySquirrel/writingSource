---
title: AOP 在 Spring 中的使用
date: 2016-11-22 15:18:04
tags: Spring
---

原文链接： [AOP在spring中的使用](http://blog.chinaunix.net/uid-20749563-id-718366.html)

面向切面程序设计(Aspect-Oriented Programming)是由Xerox Palo Alto研究中心(Xerox PARC)的Gregor kiczales等研究人员于1997年提出的一种新的程序设计思想和模型,它为开发者提供了一种模块化横切关注点的机制,并能够自动将横切关注点织入到面向对象的软件系统中,从而完成横向功能的模块化。AOP和OOP结合起来就可以很好地完成对真实系统的横向和纵向的两个维度的建模,从而更加容易地构建稳定且易于维护的软件系统。

AOP思想的本质是用一种松散耦合的方式来实现独立的关注点,通过组合这些实现来建立最终的系统。在AOP中封装横切关注点的模块单元被称为切面(Aspect),而在面向对象中封装一般关注点的模块单元被称为类。
下面对AOP的几个核心概念进行一下探讨：

### 横切关注点(Crosscutting Concern)

有一些关注点从逻辑本身上看就是独立的,比如说很多具体业务对象以及方法,而有一些关注点的逻辑本身是横向的,也就意味着其它多个独立的关注点都对其有依赖关系,比如说许多系统级代码是很多业务模块都需要包含的,这些贯穿多个模块的关注点被定义为横切关注点,比如系统日志功能，权限检查功能。横切关注点如果不被模块化将会导致软件系统在很多切面存在缺陷,典型的问题就是代码混乱和代码扩散。

### 切面(Aspect)

一系列被分解出来的关注点集就构成了切面,是一个集合。切面是AOP技术中封装的基本模块单元,利用切面这个概念我们可以方便地对横切关注点进行抽象,并且在切面里面进行一系列其它要素的定义。

### 连接点(Join Point)

连接点是在程序中定义的变量和方法

### 切点(Pointcut)

切点是用于选择连接点。它就像一个过滤器,按照满足执行内容的条件选择一些连接点。

### 通知(Advice)

一个通知就是由一个特定事件触发的程序逻辑,是能够被插入在调用者和被调用者之间的一个行为。

### 引入(Introduce)

引入是一个增加方法或域到已存在的类中的途径,它甚至允许你改变当前存在的类的显式接口,并能引入一个混合的类以实现新的接口,引入还允许你将多继承引入到一般的Java类。它一个主要的用例就是当有一个切面并让这个切面有一个运行时间接口或跨越不同的对象层次时,仍然要让应用开发者能够调用特定切面的APIs。引入还能够是一个方法,它将一个新的API绑定到一个存在的对象模型。

### 元数据(Metadata)

元数据用来描述类本身的一些附加信息,它和其描述的类捆绑在一起,可以静态地或者在运行时刻获得这些类的描述信息,甚至还可以动态地将Metadata添加到一个给定的类的实例。在EJB中就使用了大量的Metadata,Metadata也已经整合在C#和JDK 1.5中。

### 织入(Weave)

织入是实现AOP技术的关键概念之一,当我们使用关注点分离原则把横切关注点模块化成切面后,就需要考虑如何以自动化的方式把切面组织到程序相关业务类中以形成最后完整的程序,这个装配切面代码和业务核心代码的过程就称为织入。织入通常可以使用静态和动态两种方式完成,静态织入的实现可以依靠语言预处理器、扩展编译器以及扩展链接器等语言环境,而动态织入的实现就要依靠动态装载器、虚拟机等动态运行环境。

``` java
example:

public interface IBusinessLogic
{
   public void foo();
}

public class BusinessLogic implements IBusinessLogic
{
     public void foo() 
     {
       System.out.println("Inside BusinessLogic.foo()");
     }
}
```
主程序类：
``` java
public class MainApplication
{
   public static void main(String [] args)
   {
      // Read the configuration file
      ApplicationContext ctx
          = new FileSystemXmlApplicationContext("springconfig.xml");
      //Instantiate an object
      IBusinessLogic testObject = (IBusinessLogic) ctx.getBean("businesslogicbean");
      //Execute the public method of the bean (the test)
      testObject.foo();
   }
}
```
配置文件：
``` xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE beans PUBLIC "-//SPRING//DTD BEAN//EN" "http://www.springframework.org/dtd/spring-beans.dtd">

<beans>

    <!--CONFIG-->
    <bean id="businesslogicbean"
    class="org.springframework.aop.framework.ProxyFactoryBean">
        <property name="proxyInterfaces">
            <value>IBusinessLogic</value>
        </property>
        <property name="target">
            <ref local="beanTarget"/>
        </property>
    </bean>

    <!--CLASS-->
    <bean id="beanTarget"
    class="BusinessLogic"/>
</beans>
```
允许spring控制对象的初始化，就给了spring这样的一个机会：在把bean交给应用程序之前，可以给这个bean做一个J2EE所需要的所有的管理性工作
下面用用应方法跟踪切面：，方法跟踪切面是最基础的切面了！当在一个目标程序中调用一个对象的方法时.方法跟踪切面将截获对于这个方法的调用和返回信息，并且可以将这些信息用某种方式显示。在面向切面编程时由于before和after类型的执行逻辑( advice )能够在方法执行前和执行后的连接点(join point)上被触发，所以它们被用来捕获方法执行前和执行后的连接点((join point)
下面是before和after执行逻辑：
``` java
public class TracingBeforeAdvice implements MethodBeforeAdvice
{
   public void before(Method m, Object[] args, Object target) throws Throwable
   {
      System.out.println("Hello world! (by " + this.getClass().getName() + ")");
   }
}

public class TracingAfterAdvice implements AfterReturningAdvice
{
   public void afterReturning(Object object, Method m, Object[] args, Object target) throws Throwable
   {
       System.out.println("Hello world! (by " + this.getClass().getName() + ")");
   }
}
```
此时的配置spring配置文件：
``` xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE beans PUBLIC "-//SPRING//DTD BEAN//EN" "http://www.springframework.org/dtd/spring-beans.dtd">

<beans>

    <!--CONFIG-->
    <bean id="businesslogicbean"
    class="org.springframework.aop.framework.ProxyFactoryBean">
        <property name="proxyInterfaces">
            <value>IBusinessLogic</value>
        </property>
        <property name="target">
            <ref local="beanTarget"/>
        </property>
        <property name="interceptorNames">
            <list>
                <value>tracingBeforeAdvisor</value>
                <value>tracingAfterAdvisor</value>                
            </list>
        </property>
    </bean>

    <!--CLASS-->
    <bean id="beanTarget"
    class="BusinessLogic"/>

    <!-- Advisor pointcut definition for before advice -->
    <bean id="tracingBeforeAdvisor"
    class="org.springframework.aop.support.RegexpMethodPointcutAdvisor">
        <property name="advice">
            <ref local="theTracingBeforeAdvice"/>
        </property>
        <property name="pattern">
            <value>.*</value>
        </property>
    </bean>

    <!-- Advisor pointcut definition for after advice -->
    <bean id="tracingAfterAdvisor"
    class="org.springframework.aop.support.RegexpMethodPointcutAdvisor">
        <property name="advice">
            <ref local="theTracingAfterAdvice"/>
        </property>
        <property name="pattern">
            <value>.*</value>
        </property>
    </bean>

    <!--ADVICE-->
    <bean id="theTracingBeforeAdvice"
    class="TracingBeforeAdvice"/>
    <bean id="theTracingAfterAdvice"
    class="TracingAfterAdvice"/>

</beans>
```
切面重用：
可以将方法追踪切面扩展以得到更复杂一些的日志切面。由于日志切面中用到的许多元素在方法跟踪切面中已经被开发出来了，所以日志切面提供了一个很好的复用的例子。
在下面这个例子中，对于在程序在执行过程中出现的异常.日志切面扩展了方法跟踪切面以额外的显示关于异常的信息。
对于刚才我们写的程序我们需要做出一些变动，以便实现完整的日志切面。在IbusinessLogiclnterface接口和BusinessLogic实现类的bar()方法中可以抛出一个异常这个异常我们用自定义的BusinessLogicException表示。
``` java
public interface IBusinessLogic
{
   public void foo();

   public void bar() throws BusinessLogicException;
}
public class BusinessLogic implements IBusinessLogic
{
     public void foo() 
     {
       System.out.println("Inside BusinessLogic.foo()");
     }

     public void bar() throws BusinessLogicException
     {
        System.out.println("Inside BusinessLogic.bar()");
        throw new BusinessLogicException();
     }
}


public class BusinessLogicException extends Exception
{

}
public class MainApplication
{
   public static void main(String [] args)
   {
      // Read the configuration file

      ApplicationContext ctx
          = new FileSystemXmlApplicationContext("springconfig.xml");

      //Instantiate an object

      IBusinessLogic testObject = (IBusinessLogic) ctx.getBean("businesslogicbean");

      //Execute the public methods of the bean

      testObject.foo();

      try
      {
         testObject.bar();
      }
      catch(BusinessLogicException ble)
      {
         System.out.println("Caught BusinessLogicException");
      }
   }
}
```
日志切面增加了一个新的执行逻辑用以记录异常,在抛出异常之后：
``` java
public class LoggingThrowsAdvice implements ThrowsAdvice
{
   public void afterThrowing(Method method, Object[] args, Object target, Throwable subclass)
   {
      System.out.println("Logging that a " + subclass + "Exception was thrown.");
   }
}
```
after和before可以常用：
配置文件：
``` xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE beans PUBLIC "-//SPRING//DTD BEAN//EN" "http://www.springframework.org/dtd/spring-beans.dtd">

<beans>

    <!--CONFIG-->
    <bean id="businesslogicbean"
    class="org.springframework.aop.framework.ProxyFactoryBean">
        <property name="proxyInterfaces">
            <value>IBusinessLogic</value>
        </property>
        <property name="target">
            <ref local="beanTarget"/>
        </property>
        <property name="interceptorNames">
            <list>
                <value>tracingBeforeAdvisor</value>
                <value>tracingAfterAdvisor</value>
                <value>loggingThrowsAdvisor</value>
            </list>
        </property>
    </bean>

    <!--CLASS-->
    <bean id="beanTarget"
    class="BusinessLogic"/>

    <!-- Advisor pointcut definition for before advice -->
    <bean id="tracingBeforeAdvisor"
    class="org.springframework.aop.support.RegexpMethodPointcutAdvisor">
        <property name="advice">
            <ref local="theTracingBeforeAdvice"/>
        </property>
        <property name="pattern">
            <value>.*</value>
        </property>
    </bean>

    <!-- Advisor pointcut definition for after advice -->
    <bean id="tracingAfterAdvisor"
    class="org.springframework.aop.support.RegexpMethodPointcutAdvisor">
        <property name="advice">
            <ref local="theTracingAfterAdvice"/>
        </property>
        <property name="pattern">
            <value>.*</value>
        </property>
    </bean>

    <!-- Advisor pointcut definition for throws advice -->
    <bean id="loggingThrowsAdvisor"
    class="org.springframework.aop.support.RegexpMethodPointcutAdvisor">
        <property name="advice">
            <ref local="theLoggingThrowsAdvice"/>
        </property>
        <property name="pattern">
            <value>.*</value>
        </property>
    </bean>

    <!--ADVICE-->
    <bean id="theTracingBeforeAdvice"
    class="TracingBeforeAdvice"/>
    <bean id="theTracingAfterAdvice"
    class="TracingAfterAdvice"/>
    <bean id="theLoggingThrowsAdvice"
    class="LoggingThrowsAdvice"/>
</beans>
```
在上面.了解了如何使用SPring AOp功能来实现跟踪和日志切面。跟踪和日志都是相对”被动的切面，它们如果出现在程序中.并不会影响到程序的行为。跟踪和日志切面使用的都是一被动“的before和after类型的执行逻辑。但是如果你想改变你的程序的通常的行为.又该如何
呢?例如.如果你想替换掉程序中的某个方法.该怎么办？为了达到这个目标你要使用更加“主动”的around类型的执行逻辑。
``` java
public interface IBusinessLogic
{
   public void foo();
}

public class BusinessLogic implements IBusinessLogic
{
     public void foo() 
     {
       System.out.println("Inside BusinessLogic.foo()");
     }
}
public class AroundAdvice implements MethodInterceptor
{
   public Object invoke(MethodInvocation invocation) throws Throwable
   {
      System.out.println("Hello world! (by " + this.getClass().getName() + ")");
//没有invocation.proceed();不再执行下面的方法了（即BusinessLogic的foo（））

      return null;
}
}
public class MainApplication
{
   public static void main(String [] args)
   {
      // Read the configuration file

      ApplicationContext ctx
          = new FileSystemXmlApplicationContext("springconfig.xml");

      //Instantiate an object

      IBusinessLogic testObject = (IBusinessLogic) ctx.getBean("businesslogicbean");

      //Execute the public method of the bean (the test)

      testObject.foo();
   }
}
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE beans PUBLIC "-//SPRING//DTD BEAN//EN" "http://www.springframework.org/dtd/spring-beans.dtd">
<beans>

   <!--CONFIG-->
   <bean id="businesslogicbean" class="org.springframework.aop.framework.ProxyFactoryBean">
      <property name="proxyInterfaces">
         <value>IBusinessLogic</value>
      </property>
      <property name="target">
         <ref local="beanTarget"/>
      </property>
      <property name="interceptorNames">
         <list>
            <value>theAroundAdvisor</value>
         </list>
      </property>
   </bean>

   <!--CLASS-->
   <bean id="beanTarget" class="BusinessLogic"/>

   <!--ADVISOR-->
   <!--Note: An advisor assembles pointcuts and advice-->
   <bean id="theAroundAdvisor" class="org.springframework.aop.support.RegexpMethodPointcutAdvisor">
      <property name="advice">
         <ref local="theAroundAdvice"/>
      </property>
      <property name="pattern">
         <value>.*</value>
      </property>
   </bean>

   <!--ADVICE-->
   <bean id="theAroundAdvice" class="AroundAdvice"/>
</beans>
```
现在在MainAppllication对象执行后，由于我们所做的配置 BusinessLogic对象的foo()
方法将会被拦截并被替代掉，取而代之的是我们AroundAdvice对象里的invoke()方法。
前面的例子展示了BusinessLogic类的foo方法可以被我们自己写的AroundAdvice类的invoke}方法所完全替代。在AroundAdvice类的invoke方法中，并没有调用原先的foo()方法。如果你想在around类型的执行逻辑—AroundAdvice类—中执行foo()方法，你可以使用争ing提供的proceed()方法，这个方法可以由invoke()方法的参数一Methodlnvocation对象提供。
``` java
public interface IBusinessLogic
{
   public void foo();
}
public class BusinessLogic implements IBusinessLogic
{
     public void foo() 
     {
       System.out.println("Inside BusinessLogic.foo()");
     }
}
ublic class AroundAdvice implements MethodInterceptor
{
   public Object invoke(MethodInvocation invocation) throws Throwable
   {
      System.out.println("Hello world! (by " + this.getClass().getName() + ")");

      invocation.proceed();

      System.out.println("Goodbye! (by " + this.getClass().getName() + ")");

      return null;
   }
}
public class MainApplication
{
   public static void main(String [] args)
   {
      // Read the configuration file
      ApplicationContext ctx
          = new FileSystemXmlApplicationContext("springconfig.xml");

      //Instantiate an object
      IBusinessLogic testObject = (IBusinessLogic) ctx.getBean("businesslogicbean");

      //Execute the public method of the bean (the test)
      testObject.foo();
   }
}
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE beans PUBLIC "-//SPRING//DTD BEAN//EN" "http://www.springframework.org/dtd/spring-beans.dtd">
<beans>

   <!--CONFIG-->
   <bean id="businesslogicbean" class="org.springframework.aop.framework.ProxyFactoryBean">
      <property name="proxyInterfaces">
         <value>IBusinessLogic</value>
      </property>
      <property name="target">
         <ref local="beanTarget"/>
      </property>
      <property name="interceptorNames">
         <list>
            <value>theAroundAdvisor</value>
         </list>
      </property>
   </bean>

   <!--CLASS-->
   <bean id="beanTarget" class="BusinessLogic"/>

   <!--ADVISOR-->
   <!--Note: An advisor assembles pointcuts and advice-->
   <bean id="theAroundAdvisor" class="org.springframework.aop.support.RegexpMethodPointcutAdvisor">
      <property name="advice">
         <ref local="theAroundAdvice"/>
      </property>
      <property name="pattern">
         <value>.*</value>
      </property>
   </bean>

   <!--ADVICE-->
   <bean id="theAroundAdvice" class="AroundAdvice"/>
</beans>
```