# Weave-Process Documentation

Welcome to the Weave-Process documentation! This guide will help you understand, configure, and use the Weave-Process workflow automation platform.

## What is Weave-Process?

Weave-Process is a Spring Boot-based workflow execution engine that provides n8n-like workflow automation capabilities. It enables you to create, manage, and execute complex business processes through visual node-based workflows.

## Quick Navigation

```{toctree}
:maxdepth: 2
:caption: Getting Started

getting-started/overview
getting-started/installation
getting-started/quick-start
getting-started/basic-concepts
```

```{toctree}
:maxdepth: 2
:caption: User Guide

user-guide/workflow-creation
user-guide/node-configuration
user-guide/credential-management
user-guide/triggers
user-guide/monitoring
```

```{toctree}
:maxdepth: 2
:caption: Node Documentation

nodes/index
nodes/triggers/index
nodes/actions/index
nodes/integrations/index
```

```{toctree}
:maxdepth: 2
:caption: API Reference

api/admin-api
api/runner-api
api/workflow-api
api/authentication
```

```{toctree}
:maxdepth: 2
:caption: Architecture

architecture/overview
architecture/modules
architecture/database
architecture/security
architecture/deployment
```

```{toctree}
:maxdepth: 2
:caption: Development

development/setup
development/node-development
development/contributing
development/testing
```

```{toctree}
:maxdepth: 2
:caption: Operations

operations/configuration
operations/monitoring
operations/troubleshooting
operations/backup
```

## Key Features

- **Visual Workflow Design**: Create workflows using a node-based visual interface
- **Extensive Integration**: Connect to various services and APIs
- **Scalable Architecture**: Built on Spring Boot with modular design
- **Real-time Monitoring**: Track workflow execution and performance
- **Security First**: OAuth2, JWT authentication, and credential management
- **Enterprise Ready**: Designed for production environments

## Architecture Overview

Weave-Process consists of five main modules:

- **Core**: Fundamental workflow execution framework
- **Admin**: REST API for workflow management
- **Runner**: Workflow execution engine
- **Referentiel**: Data persistence layer
- **Security**: Authentication and authorization

## Getting Help

- **Documentation**: Browse through the sections above
- **API Reference**: Check the API documentation for integration details
- **Issues**: Report bugs or request features in the project repository
- **Community**: Join our community discussions

## Recent Updates

- **Node Documentation**: Added comprehensive documentation for Gmail Trigger and OpenAI Chat nodes
- **API Enhancements**: Improved workflow management endpoints
- **Security Updates**: Enhanced OAuth2 integration
- **Performance**: Optimized workflow execution engine

---

*This documentation is continuously updated. For the latest information, please refer to the project repository.*