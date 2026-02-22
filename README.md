# Orqueio Platform - Spring Boot Getting Started

A complete example demonstrating how to integrate and run the Orqueio Platform within a Spring Boot application. This project provides a ready-to-use setup for business process automation with BPMN workflows.

## Overview

Orqueio Platform is a powerful business process management (BPM) engine that enables you to design, deploy, and execute BPMN workflows. This starter project includes:

- Embedded Orqueio Engine (v2.0.0)
- Spring Boot 4.0.0 integration
- Enterprise webapps for process management
- REST API for programmatic access
- Example BPMN process (Loan Approval)
- Pre-configured development environment

## Prerequisites

- **Java**: 17 or 21
- **Spring Boot**: 4.0.0
- **Maven**: 3.6 or higher
- **IDE**: (Optional) IntelliJ IDEA, Eclipse, or VS Code

## Quick Start

### 1. Clone and Build

```bash
git clone <repository-url>
cd Orqueio-get-started-spring-boot
mvn clean install
```

### 2. Run the Application

**Option A: Using Maven**
```bash
mvn spring-boot:run
```

**Option B: Using Java**
```bash
java -jar target/orqueio-springboot-*.jar
```

### 3. Access the Platform

Once started, access the following endpoints:

- **Orqueio Webapps**: [http://localhost:8080/](http://localhost:8080/)
  - Username: `demo`
  - Password: `demo`

- **REST API**: [http://localhost:8080/engine-rest](http://localhost:8080/engine-rest)

## Configuration

### Maven Dependencies

Add the following to your `pom.xml`:

```xml
<properties>
  <spring-boot.version>4.0.0</spring-boot.version>
  <orqueio.version>2.0.0</orqueio.version>
</properties>

<dependencyManagement>
  <dependencies>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-dependencies</artifactId>
      <version>${spring-boot.version}</version>
      <type>pom</type>
      <scope>import</scope>
    </dependency>

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
</dependencies>
```

### Spring Boot Application

Create your main application class with process application support:

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

**Required Files:**
- Add an empty `processes.xml` file to `src/main/resources/META-INF/`
- This enables automatic process discovery and deployment

### Application Configuration

Configure database and security settings in `src/main/resources/application.yaml`:

- Default configuration uses an embedded H2 database
- Customize database, admin credentials, and other settings as needed

## Project Structure

```
src/
├── main/
│   ├── java/
│   │   └── io/orqueio/bpm/getstarted/
│   │       └── WebappExampleProcessApplication.java
│   └── resources/
│       ├── META-INF/
│       │   └── processes.xml
│       ├── static/
│       │   └── forms/              # HTML forms for user tasks
│       ├── bpmn/                   # BPMN process definitions
│       ├── cmmn/                   # CMMN case definitions (optional)
│       ├── dmn/                    # DMN decision tables (optional)
│       └── application.yaml
```

## Adding Process Definitions

### BPMN Processes

Place your `.bpmn` files anywhere on the classpath. They will be automatically:
- Discovered during application startup
- Deployed to the Orqueio Engine
- Registered with the process application

### User Task Forms

Add HTML forms in `src/main/resources/static/forms/`:
- Forms are automatically linked to user tasks in your BPMN processes
- Use embedded forms or external task forms

### Decision Tables (DMN)

Add `.dmn` files to the classpath for business rule automation alongside your BPMN processes.

## Features

- **Process Engine**: Full BPMN 2.0 execution engine
- **Webapps**: Tasklist, Cockpit, and Admin applications
- **REST API**: Complete process automation API
- **Auto-deployment**: Automatic discovery and deployment of process definitions
- **Embedded Database**: H2 database for quick start (production-ready databases supported)

## Default Credentials

**Web Application Login:**
- Username: `demo`
- Password: `demo`

Change these credentials in `application.yaml` for production use.

## Next Steps

1. Explore the Orqueio Webapps at [http://localhost:8080/](http://localhost:8080/)
2. Design your own BPMN processes using the Orqueio Modeler
3. Deploy additional processes by adding them to the `resources` directory
4. Customize the configuration for your environment
5. Integrate with your existing Spring Boot services

## Documentation

For more information about Orqueio Platform:
- [Official Documentation](https://orqueio.io/docs)
- [BPMN 2.0 Tutorial](https://orqueio.io/bpmn)
- [REST API Reference](https://orqueio.io/api)

## License

Check the project license file for usage terms and conditions.
 
