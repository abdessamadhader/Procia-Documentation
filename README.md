# Weave-Process Documentation

## Overview
Weave-process is a Spring Boot-based workflow execution engine that provides n8n-like workflow automation capabilities. It's designed to execute complex workflows with visual node-based processing in an enterprise environment.

## Project Structure

### Modules
The project is organized into 5 main modules:

- **core**: Fundamental workflow execution framework
- **admin**: REST API for workflow management and administration  
- **runner**: Actual workflow execution engine
- **referentiel**: Data persistence layer
- **security**: Authentication and authorization

## Quick Start

### Prerequisites
- Java 21
- Maven 3.6+
- PostgreSQL database
- RabbitMQ (optional)

### Build and Run
```bash
mvn clean install
```

## Documentation Index

- [Architecture Overview](architecture.md)
- [Module Details](modules.md)
- [API Documentation](api.md)
- [Workflow Development](workflow-development.md)
- [Node Development](node-development.md)
- [Configuration Guide](configuration.md)
- [Deployment Guide](deployment.md)
- [Troubleshooting](troubleshooting.md)

## Contributing
Please refer to the development guidelines in the [Contributing Guide](contributing.md).

## License
[Add license information here]