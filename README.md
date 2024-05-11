# 1.2-IoC-Application-Context-code

# Welcome to Spring ApplicationContext Example

This repository provides a simple example demonstrating the usage of ApplicationContext in the Spring Framework.

## Usage

### Step 1: Define a custom event and an event listener class

Create a class, like `User`, with dependencies you want to manage. Here's a snippet:

```java
import org.springframework.context.ApplicationEvent;

// Custom event class
public class CustomEvent extends ApplicationEvent {
    public CustomEvent(Object source) {
        super(source);
    }

    public String getMessage() {
        return "Custom Event Occurred!";
    }
}
```

```java

import org.springframework.context.ApplicationListener;

// Event listener class
public class CustomEventListener implements ApplicationListener<CustomEvent> {
    @Override
    public void onApplicationEvent(CustomEvent event) {
        System.out.println("Received Custom Event: " + event.getMessage());
    }
}

```

### Step 2: Define Your Class Configure with XML

In an XML file, let's say beans.xml, configure your class and its dependencies. Example:

```java
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context
                           http://www.springframework.org/schema/context/spring-context.xsd">

    <!-- Define the User bean -->
    <bean id="user" class="com.example.User">
        <constructor-arg value="John" />
    </bean>

    <!-- Define the CustomEventListener bean -->
    <bean id="customEventListener" class="com.example.CustomEventListener" />

    <!-- Enable event handling support -->
    <context:annotation-config />

</beans>
```


### Step 3: Utilize ApplicationContext
Now, leverage BeanFactory to manage your beans. Here's how:

``` java
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Main {
    public static void main(String[] args) {
        // Initialize ApplicationContext with the XML configuration file
        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");

        // Fire a custom event
        CustomEvent event = new CustomEvent(this);
        context.publishEvent(event);

        // Retrieve the User bean from the ApplicationContext
        User user = (User) context.getBean("user");

        // Use the user object
        user.greet();
    }
}

```

### Expected Output
The output of running the code will be:

```java
Received Custom Event: Custom Event Occurred!
Hello, my name is John
```
