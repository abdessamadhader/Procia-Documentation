# OpenAI Chat Node

The OpenAI Chat node enables integration with OpenAI's language models for conversational AI, text generation, and analysis tasks.

> **⚠️ Implementation Status**: This node is currently in the planning phase and not yet implemented.

## Quick Reference

- **Node Type**: `openai-chat`
- **Category**: AI & LLM
- **Credential**: OpenAI API Key
- **Models**: GPT-3.5-turbo, GPT-4, GPT-4-turbo

## Configuration

### Required Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| **Credential** | OpenAI API | OpenAI API credentials |
| **Model** | Dropdown | GPT model to use |
| **User Message** | Text | Input prompt or message |

### Optional Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| **System Message** | Text | - | System role instructions |
| **Temperature** | Number | 1.0 | Creativity level (0.0-2.0) |
| **Max Tokens** | Number | 256 | Maximum response length |
| **Top P** | Number | 1.0 | Nucleus sampling parameter |

## Usage Examples

### Basic Chat Completion

```json
{
  "credential": "openai-api",
  "model": "gpt-3.5-turbo",
  "systemMessage": "You are a helpful assistant.",
  "userMessage": "{{json.customerQuery}}",
  "temperature": 0.7
}
```

### Text Analysis

```json
{
  "credential": "openai-api",
  "model": "gpt-4",
  "systemMessage": "Analyze the sentiment of the following text.",
  "userMessage": "{{json.reviewText}}",
  "temperature": 0.2
}
```

## Output Structure

```json
{
  "response": {
    "text": "Generated response text",
    "model": "gpt-3.5-turbo",
    "usage": {
      "promptTokens": 25,
      "completionTokens": 150,
      "totalTokens": 175
    }
  },
  "metadata": {
    "finishReason": "stop",
    "created": 1640995200
  }
}
```

## Common Use Cases

1. **Customer Support**: Automated response generation
2. **Content Creation**: Blog posts, emails, summaries
3. **Text Analysis**: Sentiment analysis, categorization
4. **Translation**: Multi-language support

## Related Documentation

- [OpenAI API Setup](../../setup/openai-api.md)
- [AI Node Overview](../ai/index.md)
- [Prompt Engineering](../../guides/prompt-engineering.md)