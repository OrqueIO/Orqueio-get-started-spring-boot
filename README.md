# Orqueio Platform - Getting Started with Orqueio Platform and Spring Boot
This example demonstrates how to quickly set up and run the Orqueio Platform within a Spring Boot application.

## Prerequisites
* Java 17/21

## How is it done

1. To embed the Orqueio Engine with the Enterprise webapps and Rest API you must add the following maven coordinates 
to your `pom.xml`:

```xml
...
  <properties>
    <orqueio.version>1.0.2</orqueio.version>
  </properties>

  <dependencyManagement>
    <dependencies>
      ...
      <dependency>
        <groupId>io.orqueio.bpm</groupId>
        <artifactId>orqueio-bom</artifactId>
        <version>${orqueio.version}</version>
        <type>pom</type>
        <scope>import</scope>
      </dependency>
    </dependencies>
  </dependencyManagement>

  <dependencies>
    <dependency>
      <groupId>io.orqueio.bpm.springboot</groupId>
      <artifactId>orqueio-bpm-spring-boot-starter-webapp</artifactId>
    </dependency>
    ...
  </dependencies>
...
```
2. An "application" class annotated with `@SpringBootApplication` needs to be created. In order to have the Invoice 
process application registered, add the annotation `@EnableProcessApplication` to the same class, as well as include 
an empty `processes.xml` file to your `META-INF` folder. To ensure that all of necessary BPMN and DMN files are deployed, 
add the following code in your "application" class:

```java
@SpringBootApplication
@EnableProcessApplication
public class WebappExampleProcessApplication {

  @Autowired
  private RuntimeService runtimeService;

  public static void main(String... args) {
    SpringApplication.run(WebappExampleProcessApplication.class, args);
  }

  @EventListener
  public void processPostDeploy(PostDeployEvent event) {
    runtimeService.startProcessInstanceByKey("loanApproval");
  }
}
```

3. You can also put additional BPMN, CMMN and DMN files in your classpath, they will be automatically deployed and 
registered within the process application. Forms HTML needs to be added in the `/resources/static/forms` directory.

4. If you want your Orqueio license automatically used on Engine startup, just put the file with the name 
`orqueio-license.txt` on your classpath. 

5. Adjust the `src/main/resources/application.yaml` file according to your preferences. The default setup will use an
 embedded H2 instance.

## Run the application and use Orqueio Platform

You can build the application with `mvn clean install` and then run it with the `java -jar` command.
You can also execute the application with `mvn spring-boot:run`.

Then you can access the Orqueio Webapps in your browser: `http://localhost:8080/` (provide login/password 
from `application.yaml`, default: demo/demo). The Rest API can be available through `http://localhost:8080/engine-rest`.
 
