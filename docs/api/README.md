# API Documentation

Comprehensive API documentation for Field-Compass-IQ services.

## Base URL

```
Development: http://localhost:5000
Staging: https://staging-api.fieldcompass.com
Production: https://api.fieldcompass.com
```

## Authentication

All API endpoints require authentication using JWT Bearer tokens.

```http
Authorization: Bearer <your_jwt_token>
```

### Getting a Token

```http
POST /api/auth/login
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "password123"
}
```

## API Endpoints

### Core Services

| Service | Base Path | Description |
|---------|-----------|-------------|
| [Authentication](authentication.md) | `/api/auth` | User authentication and authorization |
| [Customers](customers.md) | `/api/customers` | Customer relationship management |
| [Work Orders](work-orders.md) | `/api/work-orders` | Work order and task management |
| [Routing](routing.md) | `/api/routes` | Route planning and optimization |
| [Users](users.md) | `/api/users` | User management |
| [Notifications](notifications.md) | `/api/notifications` | Notification management |

### Integration Endpoints

| Service | Base Path | Description |
|---------|-----------|-------------|
| [Webhooks](webhooks.md) | `/api/webhooks` | Webhook management |
| [External APIs](external-apis.md) | `/api/external` | Third-party integrations |

## Standard Response Format

### Success Response

```json
{
  "success": true,
  "data": {
    // Response data
  },
  "message": "Operation completed successfully",
  "timestamp": "2025-06-21T18:00:00Z"
}
```

### Error Response

```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Validation failed",
    "details": [
      {
        "field": "email",
        "message": "Email is required"
      }
    ]
  },
  "timestamp": "2025-06-21T18:00:00Z"
}
```

## HTTP Status Codes

| Status Code | Description |
|-------------|-------------|
| 200 | OK - Request successful |
| 201 | Created - Resource created successfully |
| 400 | Bad Request - Invalid request data |
| 401 | Unauthorized - Authentication required |
| 403 | Forbidden - Insufficient permissions |
| 404 | Not Found - Resource not found |
| 409 | Conflict - Resource conflict |
| 422 | Unprocessable Entity - Validation failed |
| 429 | Too Many Requests - Rate limit exceeded |
| 500 | Internal Server Error - Server error |

## Rate Limiting

API requests are rate limited per tenant:

- **Standard**: 1000 requests/hour
- **Premium**: 5000 requests/hour  
- **Enterprise**: 10000 requests/hour

Rate limit headers:
```http
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 999
X-RateLimit-Reset: 1640995200
```

## Pagination

List endpoints support pagination:

```http
GET /api/customers?page=1&pageSize=20&sortBy=name&sortOrder=asc
```

Response:
```json
{
  "success": true,
  "data": {
    "items": [...],
    "pagination": {
      "page": 1,
      "pageSize": 20,
      "totalItems": 150,
      "totalPages": 8,
      "hasNext": true,
      "hasPrevious": false
    }
  }
}
```

## Filtering

Supported filter operators:

| Operator | Description | Example |
|----------|-------------|----------|
| `eq` | Equals | `status=eq:active` |
| `ne` | Not equals | `status=ne:inactive` |
| `gt` | Greater than | `createdAt=gt:2025-01-01` |
| `gte` | Greater than or equal | `priority=gte:2` |
| `lt` | Less than | `priority=lt:5` |
| `lte` | Less than or equal | `updatedAt=lte:2025-12-31` |
| `contains` | Contains text | `name=contains:john` |
| `startsWith` | Starts with | `email=startsWith:admin` |
| `in` | In list | `status=in:active,pending` |

## Versioning

API versioning is handled through URL path:

```
/api/v1/customers  # Version 1
/api/v2/customers  # Version 2
```

Current version: **v1**

## Content Types

- **Request**: `application/json`
- **Response**: `application/json`
- **File Upload**: `multipart/form-data`

## CORS

CORS is enabled for the following origins:
- `http://localhost:3000` (Development)
- `https://app.fieldcompass.com` (Production)

## WebSocket Endpoints

Real-time features use SignalR:

```
ws://localhost:5000/hubs/notifications
ws://localhost:5000/hubs/workorders
ws://localhost:5000/hubs/routes
```

## SDK and Client Libraries

### Official SDKs
- **TypeScript/JavaScript**: `@field-compass/api-client`
- **.NET**: `FieldCompass.Api.Client`
- **Python**: `field-compass-python`

### Installation

```bash
# TypeScript/JavaScript
npm install @field-compass/api-client

# .NET
dotnet add package FieldCompass.Api.Client

# Python
pip install field-compass-python
```

## Testing

### Postman Collection

Import our Postman collection:
```
https://api.fieldcompass.com/postman/collection.json
```

### Test Environment

Use our test environment for development:
```
Base URL: https://test-api.fieldcompass.com
Test Tenant: test-tenant-001
Test User: test@fieldcompass.com / password123
```

## Support

- **Documentation**: [docs.fieldcompass.com](https://docs.fieldcompass.com)
- **API Status**: [status.fieldcompass.com](https://status.fieldcompass.com)
- **Support**: [support@fieldcompass.com](mailto:support@fieldcompass.com)
