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







import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.HttpHeaders;
import org.springframework.http.ResponseEntity;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.*;

@Controller
@RequestMapping("/xapi")
public class PassThroughController {
    private final ProcessAPIService processAPIService;

    @Autowired
    public PassThroughController(ProcessAPIService processAPIService) {
        this.processAPIService = processAPIService;
    }

    @RequestMapping(value = "/{endpoint}", method = RequestMethod.POST)
    @ResponseBody
    public ResponseEntity<String> handleRequest(@PathVariable("endpoint") String endpoint,
                                                @RequestParam("param1") String param1,
                                                @RequestParam("param2") int param2,
                                                @RequestBody RequestBodyType requestBody,
                                                @RequestHeader(HttpHeaders.AUTHORIZATION) String authorizationHeader) {
        // Map the headers to the corresponding process API request objects
        HttpHeaders processAPIHeaders = new HttpHeaders();
        processAPIHeaders.set(HttpHeaders.AUTHORIZATION, authorizationHeader);

        // Map the parameters and headers to the corresponding process API request objects
        ProcessAPIRequest processAPIRequest = new ProcessAPIRequest();
        processAPIRequest.setParam1(param1);
        processAPIRequest.setParam2(param2);
        processAPIRequest.setRequestBody(requestBody);
        processAPIRequest.setHeaders(processAPIHeaders);

        // Invoke the appropriate process API using the mapped request
        ProcessAPIResponse response = processAPIService.invokeProcessAPI(endpoint, processAPIRequest);

        // Map the response from the process API to the pass-through API response
        ResponseEntity<String> passThroughResponse = ResponseEntity.ok(response.getResponseBody());

        return passThroughResponse;
    }
}
import org.springframework.beans.factory.annotation.Value;
import org.springframework.http.HttpHeaders;
import org.springframework.http.ResponseEntity;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.*;

@Controller
@RequestMapping("/xapi")
public class PassThroughController {
    private final ProcessAPIService processAPIService;
    private final String apiKey; // Assuming the API key is stored in a configuration file or environment variable

    public PassThroughController(ProcessAPIService processAPIService, @Value("${api.key}") String apiKey) {
        this.processAPIService = processAPIService;
        this.apiKey = apiKey;
    }

    @RequestMapping(value = "/{endpoint}", method = RequestMethod.POST)
    @ResponseBody
    public ResponseEntity<String> handleRequest(@PathVariable("endpoint") String endpoint,
                                                @RequestParam("param1") String param1,
                                                @RequestParam("param2") int param2,
                                                @RequestBody RequestBodyType requestBody,
                                                @RequestHeader(HttpHeaders.AUTHORIZATION) String authorizationHeader) {
        // Validate the API key in the Authorization header
        if (!authorizationHeader.equals("Bearer " + apiKey)) {
            return ResponseEntity.status(HttpStatus.UNAUTHORIZED).build();
        }

        // Map the parameters and headers to the corresponding process API request objects
        ProcessAPIRequest processAPIRequest = new ProcessAPIRequest();
        processAPIRequest.setParam1(param1);
        processAPIRequest.setParam2(param2);
        processAPIRequest.setRequestBody(requestBody);

        // Invoke the appropriate process API using the mapped request
        ProcessAPIResponse response = processAPIService.invokeProcessAPI(endpoint, processAPIRequest);

        // Map the response from the process API to the pass-through API response
        ResponseEntity<String> passThroughResponse = ResponseEntity.ok(response.getResponseBody());

        return passThroughResponse;
    }
}
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.ResponseEntity;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.*;

@Controller
@RequestMapping("/xapi")
public class PassThroughController {
    private final ProcessAPIService processAPIService;

    @Autowired
    public PassThroughController(ProcessAPIService processAPIService) {
        this.processAPIService = processAPIService;
    }

    @RequestMapping(value = "/{endpoint}", method = RequestMethod.POST)
    @ResponseBody
    public ResponseEntity<String> handleRequest(@PathVariable("endpoint") String endpoint,
                                                @RequestParam("param1") String param1,
                                                @RequestParam("param2") int param2,
                                                @RequestBody RequestBodyType requestBody) {
        // Map the parameters to the corresponding process API request objects
        ProcessAPIRequest processAPIRequest = new ProcessAPIRequest();
        processAPIRequest.setParam1(param1);
        processAPIRequest.setParam2(param2);
        processAPIRequest.setRequestBody(requestBody);

        // Invoke the appropriate process API using the mapped request
        ProcessAPIResponse response = processAPIService.invokeProcessAPI(endpoint, processAPIRequest);

        // Map the response from the process API to the pass-through API response
        ResponseEntity<String> passThroughResponse = ResponseEntity.ok(response.getResponseBody());

        return passThroughResponse;
    }
}



import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import javax.servlet.http.HttpServletRequest;
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Paths;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.databind.JsonNode;

@RestController
public class GatewayController {
    
    private static final String JSON_FILE_PATH = "path/to/json/file.json";

    @PostMapping("/{papi}/**")
    public String handleRequest(@PathVariable("papi") String papi, HttpServletRequest request, @RequestBody String requestBody) throws IOException {
        String endpoint = extractEndpoint(request);
        String artifactId = extractArtifactIdFromRequest(requestBody);
        JsonNode matchedObject = findMatchingObject(artifactId);
        String downstreamUrl = matchedObject.get("downstream-url").asText();
        String methodPath = matchedObject.get("methods").get("path").asText();
        String uri = downstreamUrl + methodPath;
        return "API Name: " + papi + ", Endpoint: " + endpoint + ", Downstream URI: " + uri;
    }

    private String extractEndpoint(HttpServletRequest request) {
        String requestURI = request.getRequestURI();
        String endpoint = requestURI.substring(requestURI.indexOf("/", 1));
        return endpoint;
    }

    private String extractArtifactIdFromRequest(String requestBody) throws IOException {
        ObjectMapper objectMapper = new ObjectMapper();
        JsonNode jsonNode = objectMapper.readTree(requestBody);
        String artifactId = jsonNode.get("artifactId").asText();
        return artifactId;
    }

    private JsonNode findMatchingObject(String artifactId) throws IOException {
        byte[] jsonData = Files.readAllBytes(Paths.get(JSON_FILE_PATH));
        ObjectMapper objectMapper = new ObjectMapper();
        JsonNode jsonNode = objectMapper.readTree(jsonData);
        JsonNode matchedObject = null;
        
        // Iterate through the JSON objects and find the matching artifact ID
        for (JsonNode node : jsonNode) {
            if (node.get("artifactId").asText().equals(artifactId)) {
                matchedObject = node;
                break;
            }
        }
        
        return matchedObject;
    }
}





6th july ashrita 


@RestController
public class MyController {
    private final ArtifactService artifactService;

    public MyController(ArtifactService artifactService) {
        this.artifactService = artifactService;
    }

    @GetMapping("/artifacts/{id}")
    public ResponseEntity<Artifact> getArtifactById(@PathVariable String id) {
        try {
            Artifact artifact = artifactService.getArtifactById(id);
            if (artifact != null) {
                return ResponseEntity.ok(artifact);
            } else {
                return ResponseEntity.notFound().build();
            }
        } catch (IOException e) {
            return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).build();
        }
    }
}

import com.fasterxml.jackson.databind.ObjectMapper;

import java.io.IOException;
import java.io.InputStream;
import java.util.Arrays;
import java.util.List;
import java.util.Optional;

public class ArtifactService {
    private static final String ARTIFACTS_FILE = "artifacts.json";

    public Artifact getArtifactById(String artifactId) throws IOException {
        ObjectMapper objectMapper = new ObjectMapper();
        InputStream inputStream = getClass().getClassLoader().getResourceAsStream(ARTIFACTS_FILE);

        List<Artifact> artifacts = Arrays.asList(objectMapper.readValue(inputStream, Artifact[].class));

        Optional<Artifact> optionalArtifact = artifacts.stream()
                .filter(artifact -> artifact.getId().equals(artifactId))
                .findFirst();

        return optionalArtifact.orElse(null);
    }
}




public class Artifact {
    private String id;
    private String name;
    private String field1;
    private String field2;

    // Getters and setters
}







ashritha 


public class ArtifactInfo {
    @JsonProperty("API_name")
    private String apiName;
    @JsonProperty("artifact-id")
    private String artifactId;
    private String stash;
    private String description;
    @JsonProperty("methods")
    private List<Method> methods;
    @JsonProperty("downstream-url")
    private String downstreamUrl;

    // Getters and setters
}


public class Method {
    private String name;
    private String path;
    private String description;

    // Getters and setters
}


ArtifactInfo artifactInfo = apis.getArtifactInfoMap().get("bscustonbrdaqat-xapi");

ObjectMapper objectMapper = new ObjectMapper();
Apis apis = objectMapper.readValue(jsonString, Apis.class);


public class Method {
    private String name;
    private String path;
    private String description;

    // Getters and setters
}



eshu 7 july

jsonreador class 




import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

import java.io.IOException;

@RestController
public class ApiController {

    private final JSONReader jsonReader;

    public ApiController(JSONReader jsonReader) {
        this.jsonReader = jsonReader;
    }

    @GetMapping("/api/{artifactId}/downstream-url")
    public String getDownstreamURL(@PathVariable String artifactId) {
        try {
            return jsonReader.getDownstreamURL(artifactId);
        } catch (IOException e) {
            // Handle the exception
            return null; // or return an error response
        }
    }
}




api controller class


import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

import java.io.IOException;

@RestController
public class ApiController {

    private final JSONReader jsonReader;

    public ApiController(JSONReader jsonReader) {
        this.jsonReader = jsonReader;
    }

    @GetMapping("/api/{artifactId}/downstream-url")
    public String getDownstreamURL(@PathVariable String artifactId) {
        try {
            String downstreamURL = jsonReader.getDownstreamURL(artifactId);
            if (downstreamURL != null) {
                return "Downstream URL: " + downstreamURL;
            } else {
                return "Artifact ID not found.";
            }
        } catch (IOException e) {
            // Handle the exception
            return "Error occurred while retrieving the downstream URL.";
        }
    }
}
