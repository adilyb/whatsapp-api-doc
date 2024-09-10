Here’s an updated version of the documentation with a **Pricing** section:

---

# WhatsApp Business API Documentation

## Introduction
The **WhatsApp Business API** enables businesses to send messages to customers at scale, automate communication flows, and interact with users in a secure and reliable way. This API is used for customer engagement, support, and transaction notifications.

---

## Table of Contents
1. [Getting Started](#getting-started)
2. [API Authentication](#api-authentication)
3. [Sending Messages](#sending-messages)
   - [Text Messages](#text-messages)
   - [Media Messages](#media-messages)
   - [Interactive Messages](#interactive-messages)
4. [Message Templates](#message-templates)
5. [Webhooks](#webhooks)
6. [Rate Limits](#rate-limits)
7. [Error Handling](#error-handling)
8. [Pricing](#pricing)
9. [Security and Compliance](#security-and-compliance)
10. [FAQ](#faq)
11. [Useful Resources](#useful-resources)

---

## 1. Getting Started

### Prerequisites
- **Business Manager Account**: You need a Facebook Business Manager account.
- **WhatsApp Business Account**: Create a WhatsApp Business account in Facebook Business Manager.
- **Phone Number**: A phone number not used on the WhatsApp mobile app.
- **Access to the WhatsApp Business API**: Sign up for WhatsApp Business API access through a Business Solutions Provider (BSP) or by Facebook directly.

### Installation
To install the WhatsApp Business API on your server, follow these steps:

1. **Set up Docker**: WhatsApp API runs inside Docker containers. Install Docker on your server.
2. **Environment Setup**: Prepare your environment variables for your database, API key, and encryption key.
3. **Launch Docker Containers**: Use Docker Compose to bring up the WhatsApp containers.
   ```bash
   docker-compose up -d
   ```

4. **Verify Installation**: Use the health check endpoint:
   ```
   GET https://your-api-url:your-port/v1/health
   ```

---

## 2. API Authentication

You must authenticate each API request using an access token obtained from Facebook's Graph API.

### Steps:
1. Get an OAuth token:
   ```bash
   curl -X POST \
     "https://graph.facebook.com/v10.0/oauth/access_token?grant_type=client_credentials&client_id=YOUR_APP_ID&client_secret=YOUR_APP_SECRET"
   ```

2. Include this token in your API requests using the Authorization header:
   ```
   Authorization: Bearer YOUR_ACCESS_TOKEN
   ```

---

## 3. Sending Messages

### Text Messages
To send a simple text message to a customer:
```bash
POST /v1/messages
{
   "to": "PHONE_NUMBER",
   "type": "text",
   "text": {
       "body": "Hello, this is a test message!"
   }
}
```

### Media Messages
You can send images, audio, or video by attaching media URLs.

**Example: Send an Image**
```bash
POST /v1/messages
{
   "to": "PHONE_NUMBER",
   "type": "image",
   "image": {
       "link": "https://example.com/image.jpg"
   }
}
```

### Interactive Messages
Interactive messages include buttons, lists, or replies.

**Example: Button Template**
```bash
POST /v1/messages
{
   "to": "PHONE_NUMBER",
   "type": "interactive",
   "interactive": {
       "type": "button",
       "body": {
           "text": "Choose an option"
       },
       "action": {
           "buttons": [
               {
                   "type": "reply",
                   "reply": {
                       "id": "001",
                       "title": "Option 1"
                   }
               },
               {
                   "type": "reply",
                   "reply": {
                       "id": "002",
                       "title": "Option 2"
                   }
               }
           ]
       }
   }
}
```

---

## 4. Message Templates

To send messages outside of the 24-hour customer service window, you must use pre-approved message templates.

### Create Message Template
1. Log in to Facebook Business Manager.
2. Go to **WhatsApp Manager** > **Message Templates** > **Create Template**.
3. Submit the template for approval.

**Example: Template Message**
```bash
POST /v1/messages
{
   "to": "PHONE_NUMBER",
   "type": "template",
   "template": {
       "namespace": "your_namespace",
       "name": "template_name",
       "language": {
           "code": "en_US"
       },
       "components": [
           {
               "type": "body",
               "parameters": [
                   {
                       "type": "text",
                       "text": "Hello!"
                   }
               ]
           }
       ]
   }
}
```

---

## 5. Webhooks

Webhooks are used to receive messages and updates from WhatsApp. You need to configure a webhook URL to listen to events.

### Webhook Setup
1. **Create a Webhook URL**: Deploy a publicly accessible endpoint on your server.
2. **Configure Webhooks on Facebook**: Subscribe your app to the relevant WhatsApp events in Facebook Business Manager.

### Example Event Payload
```json
{
  "contacts": [{
    "profile": {
      "name": "John Doe"
    },
    "wa_id": "123456789"
  }],
  "messages": [{
    "from": "123456789",
    "id": "message_id",
    "timestamp": "timestamp",
    "type": "text",
    "text": {
      "body": "Hello!"
    }
  }]
}
```

---

## 6. Rate Limits

Each WhatsApp Business Account is subject to message rate limits. Rate limits vary based on your account tier (e.g., 1,000, 10,000, or 100,000 messages per day).

To check your rate limits:
```bash
GET /v1/account/limits
```

---

## 7. Error Handling

The WhatsApp Business API returns detailed error messages when a request fails. Common error codes:

- `400`: Bad Request - Invalid request format
- `401`: Unauthorized - Invalid or missing authentication
- `403`: Forbidden - Permission denied
- `429`: Too Many Requests - Rate limits exceeded
- `500`: Internal Server Error

### Example Error Response:
```json
{
   "errors": [
      {
         "code": 400,
         "title": "Bad Request",
         "detail": "Invalid phone number format"
      }
   ]
}
```

---

## 8. Pricing

WhatsApp Business API pricing varies by region and depends on message categories:

1. **Session Messages (Customer-Initiated)**:
   - First 24-hour session is free. After that, session messages are charged per 24-hour period based on country and region.

2. **Template Messages (Business-Initiated)**:
   - Template messages are charged at a rate that varies by country or region. These messages are used for sending notifications or alerts outside the 24-hour service window.

To check the latest pricing details:
- Visit the [Facebook Business Pricing Page](https://www.facebook.com/business/m/whatsapp/pricing).

---

## 9. Security and Compliance

WhatsApp has strict guidelines for business messaging. Ensure compliance with:

- **End-to-end encryption**: Messages are encrypted, and WhatsApp cannot read them.
- **Customer Opt-in**: Businesses must get explicit customer consent before sending messages.
- **Data Retention**: Ensure compliance with GDPR or other local privacy regulations.

---

## 10. FAQ

### How do I get started with WhatsApp Business API?
You need to register a WhatsApp Business Account through Facebook Business Manager and set up the API on your server.

### Can I send messages without customer consent?
No, you need explicit opt-in from customers.

### What types of messages can I send?
You can send text, media, and interactive messages, as well as use pre-approved message templates for notifications.

---

## 11. Useful Resources

- [WhatsApp Business API Documentation](https://developers.facebook.com/docs/whatsapp)
- [Facebook Graph API](https://developers.facebook.com/docs/graph-api)
- [Official WhatsApp Business Site](https://www.whatsapp.com/business)

---
