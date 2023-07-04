- 👋 Hi, I’m @koppueshu149
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...

<!---
koppueshu149/koppueshu149 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.ViewResolver;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;
import org.springframework.web.servlet.view.velocity.VelocityConfigurer;
import org.springframework.web.servlet.view.velocity.VelocityViewResolver;

@Configuration
public class GatewayConfig implements WebMvcConfigurer {

    @Bean
    public ViewResolver viewResolver() {
        VelocityViewResolver resolver = new VelocityViewResolver();
        resolver.setPrefix("/templates/");
        resolver.setSuffix(".vm");
        resolver.setCache(false);
        return resolver;
    }

    @Bean
    public VelocityConfigurer velocityConfigurer() {
        VelocityConfigurer configurer = new VelocityConfigurer();
        configurer.setResourceLoaderPath("classpath:/templates/");
        return configurer;
    }
}






this is mine ..

plugins {
    id 'org.springframework.boot' version '2.5.2'
    id 'io.spring.dependency-management' version '1.0.11.RELEASE'
    id 'java'
}

group = 'com.example'
version = '0.0.1-SNAPSHOT'
sourceCompatibility = '11'

repositories {
    mavenCentral()
}

dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-web'
    implementation 'org.springframework.cloud:spring-cloud-starter-gateway'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
}


User.java

package com.example.model;

public class User {
    private Long id;
    private String name;
    private String email;

    public User() {
    }

    public User(Long id, String name, String email) {
        this.id = id;
        this.name = name;
        this.email = email;
    }

    // Getters and setters
}


UserController.java
package com.example.controller;

import com.example.model.User;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/users")
public class UserController {

    @GetMapping("/{id}")
    public User getUserById(@PathVariable Long id) {
        User user = new User(id, "John Doe", "johndoe@example.com");
        return user;
    }

    @PostMapping
    public User createUser(@RequestBody User user) {
        // Logic to create the user in the database
        // For simplicity, let's assume the user is created and returned as is
        return user;
    }
}


GatewayConfig.java
package com.example.gateway;

import org.springframework.cloud.gateway.route.RouteLocator;
import org.springframework.cloud.gateway.route.builder.RouteLocatorBuilder;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class GatewayConfig {

    @Bean
    public RouteLocator customRouteLocator(RouteLocatorBuilder builder) {
        return builder.routes()
                .route("user-service", r -> r.path("/users/**")
                        .uri("http://localhost:8080"))
                .build();
    }
}

Application.java
package com.example;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;

@SpringBootApplication
@EnableDiscoveryClient
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}




Updated user.java
public class User {
    private Long id;
    private String name;
    private String email;

    public User() {
    }

    public User(Long id, String name, String email) {
        this.id = id;
        this.name = name;
        this.email = email;
    }

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }
}





this us ashritha 

@Controller
public class GatewayController {

    @Autowired
    private FirstApiService firstApiService;

    @Autowired
    private SecondApiService secondApiService;

    @GetMapping("/api/data")
    @ResponseBody
    public String getData() {
        // Call the first API and retrieve data
        String firstApiResponse = firstApiService.getData();
        
        // Call the second API and retrieve data
        String secondApiResponse = secondApiService.getData();

        // Prepare the response string
        String response = prepareResponse(firstApiResponse, secondApiResponse);

        return response;
    }

    private String prepareResponse(String firstApiResponse, String secondApiResponse) {
        // Combine the data from both APIs into a single response string
        String response = "First API Response: " + firstApiResponse + "\n";
        response += "Second API Response: " + secondApiResponse;

        return response;
    }
}




@Controller
public class GatewayController {

    @Autowired
    private DataService dataService;

    @GetMapping("/api/data/{id}")
    @ResponseBody
    public String getData(@PathVariable String id) {
        DataModel data = dataService.getDataById(id);

        // Prepare the response string
        String response = prepareResponse(data);

        return response;
    }

    private String prepareResponse(DataModel data) {
        // Build the response string using the data
        StringBuilder responseBuilder = new StringBuilder();
        responseBuilder.append("Data ID: ").append(data.getId()).append("\n");
        responseBuilder.append("Data Name: ").append(data.getName()).append("\n");
        // Add more fields as needed

        return responseBuilder.toString();
    }
}





@PostMapping("/api/data")
public ResponseEntity<String> processData(@RequestBody String requestBody) {
    // Perform validation on the request body
    if (requestBody == null || requestBody.isEmpty()) {
        return ResponseEntity.badRequest().body("Invalid request body");
    }

    // Parse the request body into a JSON object
    JsonObject jsonObject;
    try {
        jsonObject = new JsonParser().parse(requestBody).getAsJsonObject();
    } catch (JsonSyntaxException e) {
        return ResponseEntity.badRequest().body("Invalid JSON format");
    }

    // Extract data from the JSON object
    String name = jsonObject.get("name").getAsString();
    int age = jsonObject.get("age").getAsInt();

    // Pass the data to other components for further processing
    dataService.processData(name, age);

    // Return a response
    return ResponseEntity.ok("Data processed successfully");
}




4th july ashritha 

@RestController
public class GatewayController {

    @Autowired
    private RestTemplate restTemplate;

    @GetMapping("/api/data")
    public String getData() {
        // Call the first API
        String firstAPIUrl = "http://localhost:8080/api/first";
        ResponseEntity<String> firstAPIResponse = restTemplate.getForEntity(firstAPIUrl, String.class);
        String firstAPIData = firstAPIResponse.getBody();

        // Call the second API
        String secondAPIUrl = "http://localhost:8080/api/second";
        ResponseEntity<String> secondAPIResponse = restTemplate.getForEntity(secondAPIUrl, String.class);
        String secondAPIData = secondAPIResponse.getBody();

        // Process the data or combine the responses
        String combinedData = firstAPIData + " " + secondAPIData;

        // Render the combined data using Apache Velocity template
        // ...

        return combinedData;
    }
}
