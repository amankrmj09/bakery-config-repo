# 🧁 Config Repo

![Configuration](https://img.shields.io/badge/Config-Repository-blue.svg)
![YAML](https://img.shields.io/badge/Format-YAML-brightgreen.svg)

Welcome to the **Config Repo**, the central configuration hub of the Shah's Bakery Microservice Platform.

## 🎯 Purpose
The Config Repo is a backing Git repository that stores centralized configuration properties (like `.yml` and `.properties` files) for all microservices in the platform. These files are served dynamically by the **Config Server**.

## 🛠️ Architecture
- **Format:** YAML / Properties
- **Integration:** Spring Cloud Config Server

## 🚀 Getting Started

### Managing Configurations
1. Update the `.yml` files in this directory to alter service configurations.
2. Commit and push the changes.
3. The Spring Cloud Config Server will automatically detect and serve these changes to the connected microservices.

## 🔗 Related Links
- [Main Platform README](../README.md)
