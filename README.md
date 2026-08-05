# Spring Cloud Config Repository

Welcome to the central configuration repository for the Bakery microservices ecosystem. This repository is managed by a Spring Cloud Config Server and provides a unified, version-controlled location for all externalized configuration.

## 1. Project Directory Structure

```
config-repo/
├── .gitignore                      # Git ignore file
├── README.md                       # Project documentation & usage guide
├── API_REFERENCE.md                # Config Server & Gateway API reference guide
├── application.yml                 # Global shared configuration for all microservices
├── bakery-api-gateway.yml          # Spring Cloud Gateway routing, rate-limiting & security configs
├── bakery-auth-service.yml         # Auth service security, JWT, Redis & Kafka configurations
├── bakery-cart-service.yml         # Cart limits, idle timeout & Feign client configs
├── bakery-engagement-service.yml   # MongoDB, Elasticsearch, Kafka & Eureka client configs
├── bakery-eureka-server.yml        # Service discovery server configuration & eviction settings
├── bakery-notification-service.yml # Kafka consumers & Brevo email template ID mappings
├── bakery-order-service.yml        # Order delivery limits, tax, discount & Kafka topic configs
├── bakery-payment-service.yml      # Payment gateway toggles, transaction limits & retry policies
└── bakery-product-service.yml      # Inventory thresholds, image uploads & Kafka configuration
```

## 2. API Reference

For details on how configuration endpoints are served by the Spring Cloud Config Server, as well as the API Gateway routes and Actuator management endpoints configured across the system, see [API_REFERENCE.md](file:///d:/dev_space/bakery/config-repo/API_REFERENCE.md).

## 3. What This Repository Holds

This repository holds all the external configuration files for the various microservices in our system. The configurations are written in **YAML** (`.yml`) format. Storing configurations here instead of within individual application codebases allows us to:
- Manage environment-specific configurations independently of application code.
- Dynamically update configurations without rebuilding or restarting applications (with Spring Cloud Bus).
- Keep a complete version history of all configuration changes.

## 4. Naming Conventions

The Config Server resolves configuration files based on the client application's name and its active profile(s). We follow strict naming conventions to ensure configurations are correctly applied:

- **`application.yml`**: This is the default configuration file. Properties defined here are shared across **all** microservices regardless of their application name. This is ideal for global settings (e.g., Eureka server URLs, standard logging formats).
- **`[service-name].yml`**: This contains the default configurations for a specific service. For example, `cart-service.yml` applies only to the application named `cart-service`. It overrides properties in `application.yml`.
- **`[service-name]-[profile].yml`**: This specifies configurations for a specific service in a specific environment (profile). For example, `cart-service-prod.yml` applies to the `cart-service` when it is running with the `prod` profile active. Properties here override those in both `[service-name].yml` and `application.yml`.

## 5. Environment Profiles

Profiles are a core Spring Framework feature that allows us to segregate application configuration and make it available only in certain environments (e.g., `dev`, `qa`, `staging`, `prod`).

- When a microservice starts up, it declares its active profile (usually via an environment variable like `SPRING_PROFILES_ACTIVE=prod` or a command-line argument).
- The Config Server uses this profile to fetch the appropriate configuration file.
- Profiles allow you to define different database URLs, logging levels, or feature toggles for local development versus production without changing any code.
- If a service activates multiple profiles (e.g., `dev,db-migration`), the Config Server will fetch configurations for both, applying overrides based on the order of specification.

## 6. Adding a New Configuration File

When a new microservice is created or a new environment is introduced, you will need to add new configuration files.

### Steps to Add a New Configuration:
1. **Create the file**: In the root of this repository, create a new YAML file following the naming conventions above (e.g., `new-service-dev.yml`).
2. **Define properties**: Add the necessary Spring Boot and custom application properties in YAML format.
3. **Commit and Push**: Commit the new file to the main branch and push it to the remote Git repository. 
   ```bash
   git add new-service-dev.yml
   git commit -m "Add dev configuration for new-service"
   git push origin main
   ```
4. **Config Server Resolution**: 
   - The Spring Cloud Config Server continuously monitors this Git repository (or fetches upon client request, depending on configuration).
   - When the new microservice starts, it makes an HTTP request to the Config Server, providing its application name and active profiles.
   - The Config Server clones/pulls the latest changes from this Git repository, locates the matching YAML files, merges them (giving precedence to more specific profile files), and returns the consolidated configuration to the client microservice as a JSON payload.


## 🔗 Related Links

*For overall architecture, contribution guidelines, and security policies, please refer to the main [Blu's Bakery](https://github.com/amankrmj09/Blu_s_Bakery) repository.*

