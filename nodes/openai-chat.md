# OpenAI Chat Node Documentation

## Overview
The OpenAI Chat node enables integration with OpenAI's language models (GPT-3.5, GPT-4, etc.) to perform conversational AI tasks, text generation, analysis, and other AI-powered operations within your workflows.

> **⚠️ Current Status**: OpenAI Chat node is not yet implemented in the current version of Weave-process. The system currently includes LLMWhisperer OCR integration and skeleton LangChain chat trigger implementation. This documentation serves as a specification for the planned OpenAI implementation.

## Prerequisites

### Required Credentials
To use the OpenAI Chat node, you need:

1. **OpenAI API Key**
   - Sign up at [OpenAI Platform](https://platform.openai.com/)
   - Generate an API key in your account settings
   - Set up billing and usage limits

2. **API Access**
   - Access to desired models (GPT-3.5-turbo, GPT-4, etc.)
   - Sufficient API credits/quota
   - Understanding of rate limits

## Credential Configuration

### Setting Up OpenAI Credentials

1. **Navigate to Credentials Management**
   - Go to Admin interface
   - Select "Credentials" from the menu
   - Click "Add New Credential"

2. **Configure API Key Credentials**
   ```json
   {
     "credentialType": "openaiApi",
     "name": "OpenAI API Key",
     "parameters": {
       "apiKey": "sk-your-openai-api-key",
       "organization": "org-your-organization-id",
       "baseUrl": "https://api.openai.com/v1"
     }
   }
   ```

3. **Alternative: Bearer Token Configuration**
   ```json
   {
     "credentialType": "httpBearerAuth",
     "name": "OpenAI Bearer Token",
     "parameters": {
       "token": "sk-your-openai-api-key"
     }
   }
   ```

## Node Configuration

### Basic Settings

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| **Credential** | Dropdown | Yes | Select configured OpenAI credentials |
| **Model** | Dropdown | Yes | OpenAI model to use |
| **Operation** | Dropdown | Yes | Type of operation to perform |
| **System Message** | Text | No | System role instructions |
| **User Message** | Text | Yes | User input or prompt |
| **Temperature** | Number | No | Creativity level (0.0-2.0, default: 1.0) |
| **Max Tokens** | Number | No | Maximum response length |

### Available Models

| Model | Description | Context Window | Cost |
|-------|-------------|----------------|------|
| **gpt-4** | Most capable model | 8,192 tokens | Higher |
| **gpt-4-32k** | Extended context | 32,768 tokens | Higher |
| **gpt-3.5-turbo** | Fast, cost-effective | 4,096 tokens | Lower |
| **gpt-3.5-turbo-16k** | Extended context | 16,384 tokens | Lower |

### Operations

| Operation | Description |
|-----------|-------------|
| **Chat Completion** | Generate conversational responses |
| **Text Completion** | Complete text prompts |
| **Text Analysis** | Analyze and extract insights |
| **Content Generation** | Create structured content |
| **Translation** | Translate between languages |
| **Summarization** | Summarize long text |

### Advanced Parameters

| Parameter | Type | Range | Description |
|-----------|------|-------|-------------|
| **Temperature** | Number | 0.0-2.0 | Controls randomness (0=deterministic, 2=very creative) |
| **Top P** | Number | 0.0-1.0 | Nucleus sampling parameter |
| **Frequency Penalty** | Number | -2.0-2.0 | Reduces repetitive content |
| **Presence Penalty** | Number | -2.0-2.0 | Encourages topic diversity |
| **Stop Sequences** | Array | - | Custom stop sequences |

## Example Configurations

### 1. Basic Chat Completion
```json
{
  "credential": "openai-api-key",
  "model": "gpt-3.5-turbo",
  "operation": "chatCompletion",
  "systemMessage": "You are a helpful assistant.",
  "userMessage": "{{json.userInput}}",
  "temperature": 0.7,
  "maxTokens": 150
}
```

### 2. Text Analysis
```json
{
  "credential": "openai-api-key",
  "model": "gpt-4",
  "operation": "textAnalysis",
  "systemMessage": "Analyze the following text for sentiment, key topics, and actionable insights.",
  "userMessage": "{{json.textContent}}",
  "temperature": 0.3,
  "maxTokens": 500
}
```

### 3. Content Generation
```json
{
  "credential": "openai-api-key",
  "model": "gpt-3.5-turbo",
  "operation": "contentGeneration",
  "systemMessage": "Generate professional email responses based on the input.",
  "userMessage": "Customer inquiry: {{json.customerMessage}}",
  "temperature": 0.8,
  "maxTokens": 300
}
```

### 4. Translation
```json
{
  "credential": "openai-api-key",
  "model": "gpt-3.5-turbo",
  "operation": "translation",
  "systemMessage": "Translate the following text to French. Maintain the original tone and meaning.",
  "userMessage": "{{json.originalText}}",
  "temperature": 0.2,
  "maxTokens": 200
}
```

## Output Data Structure

The OpenAI Chat node outputs the following data structure:

```json
{
  "response": {
    "text": "Generated response text",
    "finishReason": "stop",
    "model": "gpt-3.5-turbo",
    "usage": {
      "promptTokens": 25,
      "completionTokens": 150,
      "totalTokens": 175
    }
  },
  "metadata": {
    "requestId": "req_abc123",
    "created": 1640995200,
    "choices": [
      {
        "index": 0,
        "message": {
          "role": "assistant",
          "content": "Generated response text"
        },
        "finishReason": "stop"
      }
    ]
  },
  "systemInfo": {
    "model": "gpt-3.5-turbo",
    "temperature": 0.7,
    "maxTokens": 150,
    "responseTime": 1250
  }
}
```

## Workflow Integration

### Using OpenAI Chat Output

The output from OpenAI Chat can be used in subsequent workflow nodes:

```mustache
Response: {{json.response.text}}
Model Used: {{json.response.model}}
Token Usage: {{json.response.usage.totalTokens}}
```

### Message Chaining

For conversational workflows:

```mustache
System: {{json.systemMessage}}
User: {{json.userMessage}}
Assistant: {{json.response.text}}
```

### Common Use Cases

1. **Customer Support Automation**
   - Email Trigger → OpenAI Chat → Send Response

2. **Content Processing Pipeline**
   - Document Upload → OpenAI Chat (Analysis) → Database Update

3. **Translation Workflow**
   - Slack Message → OpenAI Chat (Translate) → Post to Channel

4. **Report Generation**
   - Data Query → OpenAI Chat (Summarize) → PDF Generation

## Error Handling

### Common Error Types

| Error Type | Description | Solution |
|------------|-------------|----------|
| **401 Unauthorized** | Invalid API key | Verify credentials |
| **429 Rate Limited** | Too many requests | Implement retry logic |
| **400 Bad Request** | Invalid parameters | Check input format |
| **500 Server Error** | OpenAI service issue | Retry with exponential backoff |

### Error Response Structure

```json
{
  "error": {
    "code": "invalid_api_key",
    "message": "Invalid API key provided",
    "type": "invalid_request_error"
  },
  "success": false,
  "timestamp": "2024-01-15T10:30:00Z"
}
```

## Current LLM Integration Status

### Available in Current System

1. **LLMWhisperer OCR Service**
   - **Node**: `winu-nodes-base.ocr`
   - **Functionality**: Document OCR processing
   - **Credential**: `winuLlmWhisper` (Bearer Auth)
   - **API**: `https://llmwhisperer-api.us-central.unstract.com`

2. **LangChain Chat Trigger (Skeleton)**
   - **Node**: `@n8n/n8n-nodes-langchain.chatTrigger`
   - **Status**: Basic structure implemented
   - **Functionality**: Placeholder for future chat triggers

3. **AI/LLM Category Support**
   - Node categorization system ready
   - Pattern matching for AI/LLM nodes

### Reference Implementation Pattern

Based on existing Slack node (`n8n_nodes_base_slack.java`):

```java
@Step("n8n-nodes-base.openai")
public class N8n_nodes_base_openai extends AbstractStandardStep {
    
    @Autowired
    private HttpService httpService;
    
    @Override
    protected Message<?> executeNode(Node node, Message<?> input, ProcessRecord record) {
        // Get resolved parameters
        Map<String, Object> params = getResolvedParams(node, record);
        
        // Extract credentials
        String apiKey = extractCredentials(params);
        
        // Build OpenAI API request
        HttpRequest request = buildOpenAIRequest(params, apiKey);
        
        // Execute API call
        HttpResponse response = httpService.execute(request);
        
        // Process response
        return processOpenAIResponse(response, input);
    }
}
```

## Performance Considerations

### Optimization Tips

1. **Model Selection**
   - Use GPT-3.5-turbo for speed and cost efficiency
   - Reserve GPT-4 for complex reasoning tasks

2. **Token Management**
   - Monitor token usage to control costs
   - Implement token counting for budget management

3. **Caching**
   - Cache responses for repeated queries
   - Use deterministic settings (temperature=0) for cacheable requests

4. **Rate Limiting**
   - Implement exponential backoff
   - Consider request queuing for high-volume workflows

## Security Best Practices

### API Key Management

1. **Credential Storage**
   - Never hardcode API keys
   - Use the credential management system
   - Rotate keys regularly

2. **Access Control**
   - Limit API key permissions
   - Use organization-level controls
   - Monitor usage patterns

3. **Data Privacy**
   - Be aware of data retention policies
   - Consider on-premises alternatives for sensitive data
   - Implement data masking for PII

## Future Enhancements

### Planned Features

1. **Advanced Chat Features**
   - Multi-turn conversation support
   - Context memory management
   - Custom function calling

2. **Model Flexibility**
   - Support for fine-tuned models
   - Custom model endpoints
   - Model comparison features

3. **Streaming Support**
   - Real-time response streaming
   - Progress indicators
   - Partial response handling

4. **Integration Enhancements**
   - OpenAI Assistants API
   - DALL-E image generation
   - Whisper speech-to-text

## Troubleshooting

### Common Issues

1. **Authentication Errors**
   - Verify API key format and validity
   - Check organization permissions
   - Ensure sufficient credits

2. **Rate Limiting**
   - Implement retry logic with exponential backoff
   - Monitor rate limit headers
   - Consider upgrading API tier

3. **Token Limits**
   - Monitor input/output token usage
   - Implement text chunking for long inputs
   - Use appropriate models for context length

4. **Response Quality**
   - Adjust temperature and other parameters
   - Improve system message instructions
   - Use few-shot examples

### Debug Configuration

```yaml
logging:
  level:
    com.winu.process.runner.steps.openai: DEBUG
    com.winu.process.runner.services.openai: DEBUG
```

## Related Documentation

- [Node Development Guide](../development/node-development.md)
- [Credential Management](../credentials.md)
- [HTTP Service Configuration](../services/http-service.md)
- [LLM Integration Architecture](../architecture/llm-integration.md)

## API Reference

### OpenAI API Endpoints

- `POST /v1/chat/completions` - Chat completions
- `POST /v1/completions` - Text completions
- `GET /v1/models` - List available models
- `GET /v1/usage` - Check usage statistics

### Required Headers

```http
Authorization: Bearer sk-your-api-key
Content-Type: application/json
OpenAI-Organization: org-your-organization-id
```

### Example Request

```json
{
  "model": "gpt-3.5-turbo",
  "messages": [
    {"role": "system", "content": "You are a helpful assistant."},
    {"role": "user", "content": "Hello!"}
  ],
  "temperature": 0.7,
  "max_tokens": 150
}
```