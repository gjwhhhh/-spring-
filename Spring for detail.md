# Spring项目创建

工具：IDEA

## 一、普通方式

### 1.新建一个Spring项目

<img src="./typora_image/image-20201105164653064.png" alt="image-20201105164653064" style="zoom:67%;" />

项目总结构

<img src="./typora_image/image-20201105164925724.png" alt="image-20201105164925724" style="zoom:67%;" />

### 2.新建一个配置文件

对于spring的配置文件applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="
   http://www.springframework.org/schema/beans
   http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
   http://www.springframework.org/schema/aop
   http://www.springframework.org/schema/aop/spring-aop-3.0.xsd
   http://www.springframework.org/schema/tx
   http://www.springframework.org/schema/tx/spring-tx-3.0.xsd
   http://www.springframework.org/schema/context
   http://www.springframework.org/schema/context/spring-context-3.0.xsd">

</beans>

```

### 3.新建一个Category类

```java
package com.how2java.pojo;

public class Category {
    private int id;
    private String name;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}

```



### 4.修改配置文件

加入

```xml
<bean name="c" class="com.how2java.pojo.Category">
     <property name="name" value="category 1" />
</bean>
```

后

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:aop="http://www.springframework.org/schema/aop"
    xmlns:tx="http://www.springframework.org/schema/tx"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="
   http://www.springframework.org/schema/beans
   http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
   http://www.springframework.org/schema/aop
   http://www.springframework.org/schema/aop/spring-aop-3.0.xsd
   http://www.springframework.org/schema/tx
   http://www.springframework.org/schema/tx/spring-tx-3.0.xsd
   http://www.springframework.org/schema/context     
   http://www.springframework.org/schema/context/spring-context-3.0.xsd">
  
    <bean name="c" class="com.how2java.pojo.Category">
        <property name="name" value="category 1" />
    </bean>
  
</beans>
```

### 5.测试

```java
package com.how2java.test;

import com.how2java.pojo.Category;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class TestSpring {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext(
                new String[]{"applicationContext.xml"}
        );
        Category c = (Category)context.getBean("c");
        System.out.println(c.getName());

    }
}

```

### 6.创建Product

```java
package com.how2java.pojo;
 
public class Product {
 
    private int id;
    private String name;
    private Category category;
    public int getId() {
        return id;
    }
    public void setId(int id) {
        this.id = id;
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public Category getCategory() {
        return category;
    }
    public void setCategory(Category category) {
        this.category = category;
    }
}
```

### 7.修改配置文件

加入

```xml
<bean name="p" class="com.how2java.pojo.Product">
        <property name="name" value="product1" />
        <property name="category" ref="c" />
    </bean>
```

### 8.再次测试

```java
package com.how2java.test;
 
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
 
import com.how2java.pojo.Product;
 
public class TestSpring {
 
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext(new String[] { "applicationContext.xml" });
 
        Product p = (Product) context.getBean("p");
 
        System.out.println(p.getName());
        System.out.println(p.getCategory().getName());
    }
}
```

上面是第一种方式

下面用注解方式来运行

## 二、通过注解方式

### 2.1通过注解对对象注入

#### 2.1.1修改配置文件

最上面加入,

```xml
<context:annotation-config/>
```

并注释掉Product bean标签中对Category的指向，==我们通过注解来注入对象==，然后变成

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:aop="http://www.springframework.org/schema/aop"
    xmlns:tx="http://www.springframework.org/schema/tx" xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="
   http://www.springframework.org/schema/beans
   http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
   http://www.springframework.org/schema/aop
   http://www.springframework.org/schema/aop/spring-aop-3.0.xsd
   http://www.springframework.org/schema/tx
   http://www.springframework.org/schema/tx/spring-tx-3.0.xsd
   http://www.springframework.org/schema/context     
   http://www.springframework.org/schema/context/spring-context-3.0.xsd">
  	<!--给spring声明用注解配置-->
    <context:annotation-config/>
    <bean name="c" class="com.how2java.pojo.Category">
        <property name="name" value="category 1" />
    </bean>
    <bean name="p" class="com.how2java.pojo.Product">
        <property name="name" value="product1" />
<!--         <property name="category" ref="c" /> -->
    </bean>
  
</beans>
```

#### 2.1.2加上@Autowired注解

 在Product.java的category属性前加上@Autowired注解

> 也可以在setCategory方法前加上@Autowired，这样来达到相同的效果

```java
@Autowired
//或者@Resource(name="c")
private Category category;
```

测试类运行正常

![@Autowired的位置](https://stepimagewm.how2j.cn/4078.png)

### 2.2对Bean进行注解配置

bean对象本身，比如Category,Product可不可以移出applicationContext.xml配置文件，也通过注解进行呢？ 答案是可以的

#### 2.2.1修改配置

修改applicationContext.xml，什么都去掉，只新增：

`<context:component-scan base-package="com.how2java.pojo"/>`

变成

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:aop="http://www.springframework.org/schema/aop"
    xmlns:tx="http://www.springframework.org/schema/tx" xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="
   http://www.springframework.org/schema/beans
   http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
   http://www.springframework.org/schema/aop
   http://www.springframework.org/schema/aop/spring-aop-3.0.xsd
   http://www.springframework.org/schema/tx
   http://www.springframework.org/schema/tx/spring-tx-3.0.xsd
   http://www.springframework.org/schema/context     
   http://www.springframework.org/schema/context/spring-context-3.0.xsd">
    
  <!--扫描指定包下的component容器-->
    <context:component-scan base-package="com.how2java.pojo"/>
     
</beans>
```

#### 2.2.2@Component

为Product类加上@Component注解，即表明此类是bean

```java
@Component("p")

public class Product {
```


为Category 类加上@Compjavaonent注解，即表明此类是bean

```java
@Component("c")

public class Category {
 
```

另外，因为配置从applicationContext.xml中移出来了，所以属性初始化放在属性声明上进行了。

```java
private String name="product 1";

private String name="category 1";
```

 运行test，发现可以运行！

