# Authentication API

Complete authentication and authorization API reference.

## Overview

The Authentication API handles user login, logout, token management, and access control for the application.

**Base URL**: `/auth`

## Endpoints

### POST /auth/login

Authenticate a user and return access tokens.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `email` | string | Yes | User's email address |
| `password` | string | Yes | User's password |
| `remember_me` | boolean | No | Whether to extend token expiration (default: false) |

#### Request Example

```bash
curl -X POST \
  https://api.imxiaochong-sdad.com/v1/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "user@example.com",
    "password": "securePassword123",
    "remember_me": true
  }'
```

#### Response Example

```json
{
  "success": true,
  "data": {
    "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "token_type": "Bearer",
    "expires_in": 3600,
    "user": {
      "id": "user-123",
      "email": "user@example.com",
      "name": "John Doe",
      "role": "user"
    }
  },
  "meta": {
    "timestamp": "2024-01-01T00:00:00Z",
    "request_id": "req-12345"
  }
}
```

#### Error Responses

| Status | Code | Description |
|--------|------|-------------|
| 400 | `INVALID_CREDENTIALS` | Email or password is incorrect |
| 400 | `VALIDATION_ERROR` | Request validation failed |
| 429 | `RATE_LIMIT_EXCEEDED` | Too many login attempts |

### POST /auth/logout

Invalidate the current access token and log out the user.

#### Headers
- `Authorization: Bearer {access_token}` (required)

#### Request Example

```bash
curl -X POST \
  https://api.imxiaochong-sdad.com/v1/auth/logout \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
```

#### Response Example

```json
{
  "success": true,
  "data": {
    "message": "Successfully logged out"
  },
  "meta": {
    "timestamp": "2024-01-01T00:00:00Z",
    "request_id": "req-12346"
  }
}
```

### POST /auth/refresh

Refresh an expired access token using a refresh token.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `refresh_token` | string | Yes | Valid refresh token |

#### Request Example

```bash
curl -X POST \
  https://api.imxiaochong-sdad.com/v1/auth/refresh \
  -H "Content-Type: application/json" \
  -d '{
    "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
  }'
```

#### Response Example

```json
{
  "success": true,
  "data": {
    "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "token_type": "Bearer",
    "expires_in": 3600
  },
  "meta": {
    "timestamp": "2024-01-01T00:00:00Z",
    "request_id": "req-12347"
  }
}
```

### GET /auth/me

Get the current authenticated user's profile information.

#### Headers
- `Authorization: Bearer {access_token}` (required)

#### Request Example

```bash
curl -X GET \
  https://api.imxiaochong-sdad.com/v1/auth/me \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
```

#### Response Example

```json
{
  "success": true,
  "data": {
    "user": {
      "id": "user-123",
      "email": "user@example.com",
      "name": "John Doe",
      "role": "user",
      "created_at": "2024-01-01T00:00:00Z",
      "last_login": "2024-01-01T12:00:00Z",
      "permissions": ["read", "write"]
    }
  },
  "meta": {
    "timestamp": "2024-01-01T00:00:00Z",
    "request_id": "req-12348"
  }
}
```

## Code Examples

### JavaScript/Node.js

```javascript
// Login example
const loginUser = async (email, password) => {
  try {
    const response = await fetch('https://api.imxiaochong-sdad.com/v1/auth/login', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        email,
        password,
        remember_me: true
      })
    });
    
    const data = await response.json();
    
    if (data.success) {
      // Store tokens
      localStorage.setItem('access_token', data.data.access_token);
      localStorage.setItem('refresh_token', data.data.refresh_token);
      return data.data.user;
    } else {
      throw new Error(data.error.message);
    }
  } catch (error) {
    console.error('Login failed:', error);
    throw error;
  }
};

// Authenticated request example
const makeAuthenticatedRequest = async (url, options = {}) => {
  const token = localStorage.getItem('access_token');
  
  return fetch(url, {
    ...options,
    headers: {
      'Authorization': `Bearer ${token}`,
      'Content-Type': 'application/json',
      ...options.headers
    }
  });
};
```

### Python

```python
import requests
import json

class AuthClient:
    def __init__(self, base_url="https://api.imxiaochong-sdad.com/v1"):
        self.base_url = base_url
        self.access_token = None
        self.refresh_token = None
    
    def login(self, email, password, remember_me=False):
        """Login and store tokens"""
        response = requests.post(
            f"{self.base_url}/auth/login",
            json={
                "email": email,
                "password": password,
                "remember_me": remember_me
            }
        )
        
        data = response.json()
        
        if data["success"]:
            self.access_token = data["data"]["access_token"]
            self.refresh_token = data["data"]["refresh_token"]
            return data["data"]["user"]
        else:
            raise Exception(data["error"]["message"])
    
    def get_headers(self):
        """Get headers with authentication"""
        return {
            "Authorization": f"Bearer {self.access_token}",
            "Content-Type": "application/json"
        }
    
    def get_profile(self):
        """Get current user profile"""
        response = requests.get(
            f"{self.base_url}/auth/me",
            headers=self.get_headers()
        )
        
        return response.json()

# Usage
auth = AuthClient()
user = auth.login("user@example.com", "securePassword123")
profile = auth.get_profile()
```

## Security Considerations

### Token Storage
- **Client-side**: Store tokens securely (encrypted storage recommended)
- **Server-side**: Use secure, httpOnly cookies when possible
- **Mobile**: Use secure keychain/keystore

### Token Expiration
- Access tokens expire in 1 hour by default
- Refresh tokens expire in 30 days
- Implement automatic token refresh logic

### Best Practices
- Always use HTTPS for authentication requests
- Implement proper error handling for expired tokens
- Use refresh tokens to maintain user sessions
- Log out users on suspicious activity

## Rate Limiting

Authentication endpoints have specific rate limits:

| Endpoint | Limit | Window |
|----------|-------|---------|
| `/auth/login` | 5 attempts | 15 minutes |
| `/auth/refresh` | 10 requests | 1 minute |
| `/auth/logout` | No limit | - |
| `/auth/me` | 100 requests | 1 minute |

## Error Codes

| Code | Description | Resolution |
|------|-------------|------------|
| `INVALID_CREDENTIALS` | Wrong email/password | Check credentials |
| `TOKEN_EXPIRED` | Access token expired | Use refresh token |
| `TOKEN_INVALID` | Malformed or invalid token | Re-authenticate |
| `ACCOUNT_LOCKED` | Too many failed attempts | Wait or contact support |
| `REFRESH_TOKEN_EXPIRED` | Refresh token expired | Re-authenticate |