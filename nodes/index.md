# Node Documentation

This section contains comprehensive documentation for all available nodes in the Weave-Process platform.

## Node Categories

### Triggers
Trigger nodes start workflow execution based on various events and conditions.

```{toctree}
:maxdepth: 1

triggers/manual-trigger
triggers/schedule-trigger
triggers/webhook-trigger
triggers/gmail-trigger
triggers/kafka-trigger
```

### Actions
Action nodes perform specific operations within workflows.

```{toctree}
:maxdepth: 1

actions/http-request
actions/database-query
actions/file-operations
actions/email-send
```

### AI & LLM
AI and Language Model nodes for intelligent processing.

```{toctree}
:maxdepth: 1

ai/openai-chat
ai/llm-ocr
ai/langchain-chat
```

### Integrations
Integration nodes for connecting to external services.

```{toctree}
:maxdepth: 1

integrations/slack
integrations/google-drive
integrations/postgres
integrations/epm22
```

### Flow Control
Nodes for controlling workflow execution flow.

```{toctree}
:maxdepth: 1

flow-control/if-condition
flow-control/switch
flow-control/loop
flow-control/merge
```

### Utility
Utility nodes for data manipulation and processing.

```{toctree}
:maxdepth: 1

utility/set-data
utility/code-execution
utility/data-transformation
utility/split-batches
```

## Node Development

### Currently Implemented Nodes

| Node | Type | Status | Description |
|------|------|--------|-------------|
| **Manual Trigger** | Trigger | ✅ Implemented | Manually start workflows |
| **Schedule Trigger** | Trigger | ✅ Implemented | Time-based workflow execution |
| **HTTP Request** | Action | ✅ Implemented | Make HTTP API calls |
| **Postgres** | Integration | ✅ Implemented | Database operations |
| **Slack** | Integration | ✅ Implemented | Slack messaging |
| **If Condition** | Flow Control | ✅ Implemented | Conditional workflow routing |
| **Switch** | Flow Control | ✅ Implemented | Multi-branch routing |
| **Set Data** | Utility | ✅ Implemented | Data manipulation |
| **Code Execution** | Utility | ✅ Implemented | Custom JavaScript execution |
| **Split in Batches** | Utility | ✅ Implemented | Batch processing |
| **LLM OCR** | AI | ✅ Implemented | Document OCR processing |

### Planned Nodes

| Node | Type | Status | Priority | Description |
|------|------|--------|----------|-------------|
| **Gmail Trigger** | Trigger | 📋 Planned | High | Email-based triggers |
| **OpenAI Chat** | AI | 📋 Planned | High | AI-powered text processing |
| **LangChain Chat** | AI | 🚧 Skeleton | Medium | LangChain integration |
| **Webhook Trigger** | Trigger | 📋 Planned | Medium | HTTP webhook triggers |
| **File Operations** | Utility | 📋 Planned | Medium | File system operations |
| **Email Send** | Action | 📋 Planned | Medium | Email sending |
| **Google Drive** | Integration | 📋 Planned | Low | Google Drive integration |

## Node Architecture

### Base Classes

All nodes inherit from base classes that provide common functionality:

- **AbstractStandardStep**: Base class for all workflow steps
- **AbstractWorkflowProcess**: Base class for workflow processes
- **AbstractWorkflowStep**: Base class for individual steps

### Node Implementation Pattern

```java
@Step("node-type-identifier")
public class NodeImplementation extends AbstractStandardStep {
    
    @Override
    protected Message<?> executeNode(Node node, Message<?> input, ProcessRecord record) {
        // Node implementation logic
        Map<String, Object> params = getResolvedParams(node, record);
        
        // Process input and generate output
        Object result = processNode(params, input);
        
        // Return message with result
        return MessageBuilder.withPayload(result)
                .copyHeaders(input.getHeaders())
                .build();
    }
}
```

### Credential Integration

Nodes can access credentials through the credential management system:

```java
// Extract credentials from node parameters
String credentialId = (String) params.get("credential");
Map<String, Object> credentials = credentialService.getCredentials(credentialId);
```

## Common Features

### Parameter Resolution

All nodes support dynamic parameter resolution using:

- **Mustache Templates**: `{{json.fieldName}}`
- **Expression Evaluation**: JavaScript expressions
- **Context Variables**: Access to previous node outputs

### Error Handling

Nodes implement standardized error handling:

- **Functional Errors**: `Echec` exceptions for business logic errors
- **Technical Errors**: Standard exceptions for system errors
- **Retry Logic**: Configurable retry mechanisms

### Output Standardization

Node outputs follow consistent patterns:

```json
{
  "data": "Main result data",
  "metadata": {
    "nodeId": "node-identifier",
    "executionTime": 1250,
    "status": "success"
  }
}
```

## Best Practices

### Node Development

1. **Follow Naming Conventions**: Use n8n-compatible naming
2. **Implement Error Handling**: Proper exception handling
3. **Document Parameters**: Clear parameter descriptions
4. **Test Thoroughly**: Unit and integration tests
5. **Performance Optimization**: Efficient resource usage

### Configuration

1. **Use Credentials**: Never hardcode sensitive information
2. **Validate Inputs**: Check parameter formats and ranges
3. **Default Values**: Provide sensible defaults
4. **Documentation**: Clear usage instructions

### Workflow Design

1. **Node Composition**: Break complex operations into multiple nodes
2. **Error Paths**: Design workflows with error handling
3. **Monitoring**: Include logging and monitoring points
4. **Testing**: Test workflows with various scenarios

## Contributing

To contribute a new node:

1. **Design**: Create node specification
2. **Implement**: Follow the implementation pattern
3. **Test**: Write comprehensive tests
4. **Document**: Create user documentation
5. **Review**: Submit for code review

See the [Node Development Guide](../development/node-development.md) for detailed instructions.

## Support

For node-specific questions or issues:

- **Documentation**: Check the specific node documentation
- **Examples**: Look at existing node implementations
- **Community**: Join development discussions
- **Issues**: Report bugs or request features