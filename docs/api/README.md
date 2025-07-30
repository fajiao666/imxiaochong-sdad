# API Documentation

Complete API reference for all public endpoints, services, and interfaces.

## 📚 API Categories

### Core APIs
- [Authentication API](auth.md) - User authentication and authorization
- [User Management API](users.md) - User CRUD operations and profiles
- [Data API](data.md) - Core data operations and queries

### Service APIs
- [Storage API](storage.md) - File and data storage operations
- [Analytics API](analytics.md) - Data analytics and reporting
- [Notification API](notifications.md) - Push notifications and messaging

## 🚀 Quick Start

### Base URL
```
Production: https://api.imxiaochong-sdad.com/v1
Development: http://localhost:3000/api/v1
```

### Authentication
All API requests require authentication via Bearer token:

```bash
curl -H "Authorization: Bearer YOUR_TOKEN" \
     -H "Content-Type: application/json" \
     https://api.imxiaochong-sdad.com/v1/endpoint
```

### Rate Limiting
- **Rate Limit**: 1000 requests per hour per API key
- **Burst Limit**: 100 requests per minute
- **Headers**: `X-RateLimit-Limit`, `X-RateLimit-Remaining`, `X-RateLimit-Reset`

## 📋 API Standards

### Request Format
- **Content-Type**: `application/json`
- **Accept**: `application/json`
- **Encoding**: UTF-8

### Response Format
All API responses follow this structure:

```json
{
  "success": true,
  "data": {
    // Response data
  },
  "meta": {
    "timestamp": "2024-01-01T00:00:00Z",
    "version": "1.0.0",
    "request_id": "uuid-string"
  }
}
```

### Error Format
Error responses include detailed information:

```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid input parameters",
    "details": {
      "field": "email",
      "issue": "Invalid email format"
    }
  },
  "meta": {
    "timestamp": "2024-01-01T00:00:00Z",
    "request_id": "uuid-string"
  }
}
```

## 🔒 Security

### HTTPS Only
All API endpoints require HTTPS. HTTP requests are automatically redirected.

### API Key Management
- Generate API keys in the developer console
- Rotate keys regularly (recommended every 90 days)
- Use different keys for development and production

### Request Signing
For sensitive operations, requests must be signed:

```javascript
const signature = crypto
  .createHmac('sha256', secretKey)
  .update(requestBody)
  .digest('hex');
```

## 📊 Status Codes

| Code | Meaning | Description |
|------|---------|-------------|
| 200 | OK | Request successful |
| 201 | Created | Resource created successfully |
| 400 | Bad Request | Invalid request parameters |
| 401 | Unauthorized | Invalid or missing authentication |
| 403 | Forbidden | Insufficient permissions |
| 404 | Not Found | Requested resource not found |
| 429 | Too Many Requests | Rate limit exceeded |
| 500 | Internal Server Error | Server error occurred |

## 🔧 SDKs and Tools

### Official SDKs
- [JavaScript/Node.js SDK](../examples/javascript-sdk.md)
- [Python SDK](../examples/python-sdk.md)
- [Go SDK](../examples/go-sdk.md)

### Testing Tools
- [Postman Collection](../examples/postman-collection.json)
- [OpenAPI Specification](openapi.yaml)
- [Insomnia Workspace](../examples/insomnia-workspace.json)

## 📈 Versioning

We use semantic versioning for our API:
- **Major**: Breaking changes
- **Minor**: New features, backward compatible
- **Patch**: Bug fixes, backward compatible

Current version: `v1.0.0`

## 📞 Support

- **Documentation**: [docs.imxiaochong-sdad.com](https://docs.imxiaochong-sdad.com)
- **Support Email**: api-support@imxiaochong-sdad.com
- **Developer Forum**: [forum.imxiaochong-sdad.com](https://forum.imxiaochong-sdad.com)
- **Status Page**: [status.imxiaochong-sdad.com](https://status.imxiaochong-sdad.com)