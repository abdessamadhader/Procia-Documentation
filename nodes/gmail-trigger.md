# Gmail Trigger Node Documentation

## Overview
The Gmail Trigger node allows you to automatically trigger workflows when specific Gmail events occur (new emails, specific labels, etc.). This node monitors your Gmail inbox and starts workflow execution based on defined criteria.

> **⚠️ Current Status**: Gmail trigger node is not yet implemented in the current version of Weave-process. This documentation serves as a specification for the planned implementation.

## Prerequisites

### Required Credentials
To use the Gmail trigger node, you need to configure Gmail API credentials:

1. **Google Cloud Console Setup**
   - Create a project in Google Cloud Console
   - Enable Gmail API
   - Create OAuth2 credentials (Client ID and Client Secret)
   - Configure authorized redirect URIs

2. **Gmail API Scopes**
   The node requires the following Gmail API scopes:
   - `https://www.googleapis.com/auth/gmail.readonly` - Read access to Gmail
   - `https://www.googleapis.com/auth/gmail.modify` - Modify access (for marking emails as read)

## Credential Configuration

### Setting Up Gmail Credentials

1. **Navigate to Credentials Management**
   - Go to Admin interface
   - Select "Credentials" from the menu
   - Click "Add New Credential"

2. **Configure OAuth2 Credentials**
   ```json
   {
     "credentialType": "gmailOAuth2Api",
     "name": "Gmail Account",
     "parameters": {
       "clientId": "your-client-id.apps.googleusercontent.com",
       "clientSecret": "your-client-secret",
       "accessToken": "generated-during-oauth-flow",
       "refreshToken": "generated-during-oauth-flow",
       "scope": "https://www.googleapis.com/auth/gmail.readonly"
     }
   }
   ```

3. **OAuth2 Flow**
   - Click "Authorize" to start OAuth2 flow
   - Grant permission to access Gmail
   - System will automatically store access and refresh tokens

## Node Configuration

### Basic Settings

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| **Credential** | Dropdown | Yes | Select configured Gmail credentials |
| **Trigger Event** | Dropdown | Yes | Type of Gmail event to monitor |
| **Polling Interval** | Number | No | Check interval in minutes (default: 5) |
| **Max Results** | Number | No | Maximum emails to process per poll (default: 10) |

### Trigger Events

| Event Type | Description |
|------------|-------------|
| **New Email** | Triggers on any new email received |
| **New Email with Label** | Triggers on emails with specific labels |
| **New Email from Sender** | Triggers on emails from specific senders |
| **New Email with Subject** | Triggers on emails matching subject patterns |

### Advanced Filters

| Parameter | Type | Description |
|-----------|------|-------------|
| **From Address** | String | Filter by sender email address |
| **Subject Contains** | String | Filter by subject content |
| **Label** | Dropdown | Filter by Gmail label |
| **Has Attachment** | Boolean | Filter emails with/without attachments |
| **Is Unread** | Boolean | Filter read/unread emails |

## Example Configurations

### 1. Monitor New Emails from Specific Sender
```json
{
  "credential": "gmail-account",
  "triggerEvent": "newEmailFromSender",
  "fromAddress": "notifications@example.com",
  "pollingInterval": 2,
  "maxResults": 5
}
```

### 2. Monitor Emails with Specific Label
```json
{
  "credential": "gmail-account",
  "triggerEvent": "newEmailWithLabel",
  "label": "Important",
  "isUnread": true,
  "pollingInterval": 1
}
```

### 3. Monitor Order Confirmations
```json
{
  "credential": "gmail-account",
  "triggerEvent": "newEmailWithSubject",
  "subjectContains": "Order Confirmation",
  "hasAttachment": true,
  "pollingInterval": 5
}
```

## Output Data Structure

The Gmail trigger node outputs the following data structure:

```json
{
  "id": "email-message-id",
  "threadId": "thread-id",
  "subject": "Email Subject",
  "from": {
    "name": "Sender Name",
    "email": "sender@example.com"
  },
  "to": [
    {
      "name": "Recipient Name",
      "email": "recipient@example.com"
    }
  ],
  "date": "2024-01-15T10:30:00Z",
  "body": {
    "text": "Plain text content",
    "html": "HTML content"
  },
  "attachments": [
    {
      "filename": "document.pdf",
      "mimeType": "application/pdf",
      "size": 1024,
      "attachmentId": "attachment-id"
    }
  ],
  "labels": ["INBOX", "IMPORTANT"],
  "isUnread": true,
  "snippet": "Email preview text..."
}
```

## Workflow Integration

### Using Gmail Trigger Output

The output from Gmail trigger can be used in subsequent workflow nodes:

```mustache
Subject: {{json.subject}}
From: {{json.from.email}}
Body: {{json.body.text}}
```

### Common Use Cases

1. **Email Processing Pipeline**
   - Gmail Trigger → Extract Attachments → Process Documents → Send Response

2. **Notification System**
   - Gmail Trigger → Parse Content → Send Slack Notification

3. **Order Processing**
   - Gmail Trigger → Extract Order Details → Update Database → Send Confirmation

## Troubleshooting

### Common Issues

1. **Authentication Errors**
   - Verify OAuth2 credentials are correctly configured
   - Check if refresh token is still valid
   - Ensure required scopes are granted

2. **No Emails Detected**
   - Check polling interval settings
   - Verify filter criteria
   - Ensure emails match the specified conditions

3. **Rate Limiting**
   - Gmail API has rate limits (default: 1 billion quota units per day)
   - Adjust polling interval if hitting limits
   - Consider using webhooks for real-time triggers (future enhancement)

### Debug Mode

Enable debug logging to troubleshoot issues:

```yaml
logging:
  level:
    com.winu.process.runner.steps.gmail: DEBUG
```

## Future Enhancements

### Planned Features

1. **Webhook Support**
   - Real-time email notifications using Gmail Push API
   - Eliminate polling overhead

2. **Advanced Filtering**
   - Regular expression support for subject/body filtering
   - Date range filtering
   - Attachment type filtering

3. **Batch Processing**
   - Process multiple emails in single workflow execution
   - Configurable batch size

4. **Email Actions**
   - Mark emails as read/unread
   - Add/remove labels
   - Move to folders

## Related Documentation

- [Credential Management](../credentials.md)
- [Workflow Triggers](../workflow-triggers.md)
- [Node Development Guide](../development/node-development.md)
- [OAuth2 Configuration](../oauth2-setup.md)

## API Reference

### Gmail API Methods Used

- `gmail.users.messages.list()` - List messages
- `gmail.users.messages.get()` - Get message details
- `gmail.users.labels.list()` - List available labels
- `gmail.users.history.list()` - Get mailbox changes (future)

### Required Permissions

```json
{
  "scopes": [
    "https://www.googleapis.com/auth/gmail.readonly",
    "https://www.googleapis.com/auth/gmail.modify"
  ]
}
```