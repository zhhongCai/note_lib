# aop配置,根据spring-aop-4.1.xsd创建

```xml

<beans>
    <aop:config proxy-target-class="false" expose-proxy="false">
    
        <aop:pointcut id="required: The unique identifier for a pointcut." 
            expression="required: The pointcut expression, i.e execution(* com..service..*.*(..))" />
            
        <aop:advisor id="string" 
            pointcut="A pointcut expression."
            pointcut-ref="A reference to a pointcut definition." 
            advice-ref="required: A reference to an advice bean.expected type org.aopalliance.aop.Advice" 
            order="Controls the ordering of the execution of this advice when multiple advice executes at a specific joinpoint."/>
        
        <aop:aspect id="The unique identifier for an aspect."
            ref="The name of the (backing) bean that encapsulates the aspect."
            order="Controls the ordering of the execution of this aspect when multiple advice executes at a specific joinpoint.">
            <!--详见pointcut节点-->
            <aop:pointcut />
            <!--types-matching:      he AspectJ type expression that defines what types (classes) the introduction is restricted to. An example would be 'org.springframework.beans.ITestBean+-->
            <!--implement-interface: The fully qualified name of the interface that will be introduced.-->
            <!--default-impl:        The fully qualified name of the class that will be instantiated to serve as the default implementation of the introduced interface.-->
            <!--delegate-ref:        A reference to the bean that will serve as the default implementation of the introduced interface.-->
            <aop:eclare-parents 
                types-matching="string required"  
                implement-interface="string required"
                default-impl="string"
                delegate-ref="string"/>
            <!--arg-names:      The comma-delimited list of advice method argument (parameter) names that will be matched from pointcut parameters.-->
            <aop:before pointcut="The associated pointcut expression"
                pointcut-ref="The name of an associated pointcut definition"
                method="string required: The name of the method that defines the logic of the advice."
                arg-names="string"/>
            <!--配置同before-->
            <aop:after />
            <!--配置同before-->
            <aop:around />
            <aop:after-returning 
                pointcut="The associated pointcut expression"
                pointcut-ref="The name of an associated pointcut definition"
                method="string required: The name of the method that defines the logic of the advice."
                arg-names="string"
                returning="string: The name of the method parameter to which the return value must be passed."/>
            
            <aop:after-throwing 
               pointcut="The associated pointcut expression"
               pointcut-ref="The name of an associated pointcut definition"
               method="string required: The name of the method that defines the logic of the advice."
               arg-names="string"
               throwing="string: The name of the method parameter to which the thrown exception must be passed."/>
        </aop:aspect>
    </aop:config>
    
    <aop:aspectj-autoproxy 
        proxy-target-class="false: Are class-based (CGLIB) proxies to be created? By default, standard Java interface-based proxies are created."
        expose-proxy="	Indicate that the proxy should be exposed by the AOP framework as a
                      	ThreadLocal for retrieval via the AopContext class. Off by default,
                      	i.e. no guarantees that AopContext access will work.">
        <!--0个或多个include 
            Indicates that only @AspectJ beans with names matched by the (regex)
            pattern will be considered as defining aspects to use for Spring autoproxying.-->
        <aop:include name="The regular expression defining which beans are to be included in the
                           	list of @AspectJ beans; beans with names matched by the pattern will be included."/>
    </aop:aspectj-autoproxy>
    
    <!--Marks a bean definition as being a scoped proxy.
        A bean marked as such will be exposed via a proxy, with the 'real'
        bean instance being retrieved from some other source (such as a  HttpSession) as and when required.
        java:org.springframework.aop.scope.ScopedProxyFactoryBean-->
    <aop:scoped-proxy proxy-target-class="true: Are class-based (CGLIB) proxies to be created? This is the default; in order to
                                                	switch to standard Java interface-based proxies, turn this flag to false."/>

</beans>
```

## aop:scoped-proxy 怎么理解

看个例子：

```java

@Component
public class A {
    @Autowired
    private B b;
    
    public void test() {
        b.
    }
}

@Component
@Scope(value = "prototype")
public class B {
    private int count = 0;
    
    public B() {
        System.out.println("New Instance");
    }
    
    public void print() {
        System.out.println("count = " + count);
        count++;
    }
}

```

每次调用A的test方法，使用的是同个B实例,如果需要每次都创建B实例，可以使用aop:scoped-proxy:

```java

@Component
@Scope(proxyMode = ScopedProxyMode.TARGET_CLASS, value = "prototype")
public class B {
    private int count = 0;
    
    public B() {
        System.out.println("New Instance");
    }
    
    public void print() {
        System.out.println("count = " + count);
        count++;
    }
}

```
现在调用A的test方法，每次都会创建B实例 //TODO 待验证
 
# 参考：

[Spring Scoped Proxy Beans – An Alternative to Method Injection](https://shekhargulati.com/2010/10/30/spring-scoped-proxy-beans-an-alternative-to-method-injection/)