# Spring 5 MVC Application Using Java Configuration

This tutorial explains how to create a **Spring 5 MVC Application** using pure Java configuration, without relying on XML files.

---

## Prerequisites

- Java 8 or higher
- Maven or Gradle
- Spring Framework 5.x
- A development environment (e.g., IntelliJ IDEA, Eclipse, VS Code)
- Apache Tomcat or any web server

---

## Project Setup

### 1. Add Maven Dependencies

Include the following dependencies in your `pom.xml` file:

```xml
<dependencies>
    <!-- Spring Core -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>5.3.30</version>
    </dependency>
    
    <!-- Spring Web -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>5.3.30</version>
    </dependency>

    <!-- JSP support -->
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>javax.servlet-api</artifactId>
        <version>4.0.1</version>
        <scope>provided</scope>
    </dependency>

    <!-- JSTL -->
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>jstl</artifactId>
        <version>1.2</version>
    </dependency>
</dependencies>



import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.ViewResolver;
import org.springframework.web.servlet.config.annotation.EnableWebMvc;
import org.springframework.web.servlet.config.annotation.ResourceHandlerRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;
import org.springframework.web.servlet.view.InternalResourceViewResolver;

@Configuration
@EnableWebMvc
@ComponentScan(basePackages = "com.example")
public class AppConfig implements WebMvcConfigurer {

    @Bean
    public ViewResolver viewResolver() {
        InternalResourceViewResolver resolver = new InternalResourceViewResolver();
        resolver.setPrefix("/WEB-INF/views/");
        resolver.setSuffix(".jsp");
        return resolver;
    }

    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/resources/**").addResourceLocations("/resources/");
    }
}


import org.springframework.web.servlet.support.AbstractAnnotationConfigDispatcherServletInitializer;

public class AppInitializer extends AbstractAnnotationConfigDispatcherServletInitializer {

    @Override
    protected Class<?>[] getRootConfigClasses() {
        return null; // No root configuration
    }

    @Override
    protected Class<?>[] getServletConfigClasses() {
        return new Class[]{AppConfig.class};
    }

    @Override
    protected String[] getServletMappings() {
        return new String[]{"/"};
    }
}


import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class HomeController {

    @GetMapping("/")
    public String home(Model model) {
        model.addAttribute("message", "Welcome to Spring MVC Application!");
        return "home"; // Corresponds to home.jsp
    }
}


Add JSP files under /WEB-INF/views/:

home.jsp
jsp
Copy code
<!DOCTYPE html>
<html>
<head>
    <title>Spring MVC App</title>
</head>
<body>
    <h1>${message}</h1>
</body>
</html>
Directory Structure
arduino
Copy code
src/main/java
└── com.example
    ├── AppConfig.java
    ├── AppInitializer.java
    └── HomeController.java

src/main/webapp
├── WEB-INF
│   ├── views
│   │   └── home.jsp
│   └── web.xml (optional, not needed with Java config)
└── resources (static resources like CSS/JS)
Running the Application
Deploy the application on Apache Tomcat or any servlet container.
Access the application in your browser at http://localhost:8080/
