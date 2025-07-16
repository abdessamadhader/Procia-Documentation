# Gmail Trigger Node

The Gmail Trigger node monitors your Gmail inbox and automatically starts workflows when specific email events occur.

> **⚠️ Implementation Status**: This node is currently in the planning phase and not yet implemented.

## Quick Reference

- **Node Type**: `gmail-trigger`
- **Category**: Triggers
- **Credential**: Gmail OAuth2 or API Key
- **Polling**: Configurable interval (default: 5 minutes)

## Configuration

### Required Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| **Credential** | Gmail OAuth2 | Gmail API credentials |
| **Trigger Event** | Dropdown | Event type to monitor |

### Optional Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| **Polling Interval** | Number | 5 | Check interval in minutes |
| **Max Results** | Number | 10 | Maximum emails per poll |
| **From Address** | String | - | Filter by sender |
| **Subject Contains** | String | - | Filter by subject |
| **Label** | String | - | Filter by Gmail label |

## Usage Examples

### Basic New Email Trigger

```json
{
  "credential": "gmail-oauth2",
  "triggerEvent": "newEmail",
  "pollingInterval": 2
}
```

### Filter by Sender

```json
{
  "credential": "gmail-oauth2",
  "triggerEvent": "newEmailFromSender",
  "fromAddress": "orders@shop.com",
  "maxResults": 5
}
```

## Output Structure

```json
{
  "id": "email-id",
  "subject": "Email Subject",
  "from": {
    "name": "Sender Name",
    "email": "sender@example.com"
  },
  "body": {
    "text": "Plain text content",
    "html": "HTML content"
  },
  "attachments": [],
  "date": "2024-01-15T10:30:00Z"
}
```

## Related Documentation

- [Gmail API Setup](../../setup/gmail-api.md)
- [OAuth2 Configuration](../../setup/oauth2.md)
- [Trigger Nodes Overview](../triggers/index.md)