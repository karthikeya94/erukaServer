# Eureka Server

## Overview
The **Eureka Server** is the Service Discovery and Registration server for the microservices ecosystem. It acts as a central registry where all microservices (clients) register themselves. This allows services to find and communicate with each other dynamically without hardcoding hostnames and ports.

## Features
- **Service Registry**: Maintains a list of all available service instances.
- **Service Discovery**: Allows clients (like API Gateway and other microservices) to discover the network locations of service instances.
- **Health Monitoring**: Periodically receives heartbeats from registered services to ensure they are still alive. If a service fails to send a heartbeat, it is removed from the registry.
- **High Availability**: Can be configured in a cluster for redundancy (currently configured as a standalone server).

## Tech Stack
- **Java 17**
- **Spring Boot 3.5.7**
- **Spring Cloud Netflix Eureka Server**
- **Maven**

## Configuration
The service is configured in `application.yaml`. Key configurations include:

- **Server Port**: `8761`
- **Application Name**: `eureka server`
- **Standalone Mode**: Configured not to register with itself (`register-with-eureka: false`, `fetch-registry: false`).

## Getting Started

### Prerequisites
- Java 17 or higher
- Maven

### Installation
1. Clone the repository.
2. Navigate to the `eureka-server` directory.

### Running the Service
You can run the service using Maven:

```bash
mvn spring-boot:run
```

Or build the JAR and run it:

```bash
mvn clean package
java -jar target/eureka-server-0.0.1-SNAPSHOT.jar
```

The service will start on **http://localhost:8761**.

## Usage

### Accessing the Dashboard
Once the server is running, you can access the **Eureka Dashboard** at:

**[http://localhost:8761](http://localhost:8761)**

The dashboard provides a visual view of:
- Currently registered instances.
- General server information (uptime, environment).
- Instance health status.

### Registering a Service
To register a microservice with this Eureka Server, add the `spring-cloud-starter-netflix-eureka-client` dependency to the microservice and configure the Eureka client URL in its `application.yml`:

```yaml
eureka:
  client:
    service-url:
      defaultZone: http://localhost:8761/eureka/
```

## Troubleshooting
- **Service not appearing**: Ensure the microservice has the correct `defaultZone` URL and is annotated with `@EnableDiscoveryClient` (optional in recent Spring Cloud versions but good practice).
- **"EMERGENCY! EUREKA MAY BE INCORRECTLY CLAIMING INSTANCES ARE UP WHEN THEY'RE NOT"**: This warning on the dashboard usually appears in development when the renewal threshold is not met (e.g., when running a single instance). It can be ignored in a local development environment.
