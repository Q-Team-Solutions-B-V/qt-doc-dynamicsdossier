# API Overview

## Introduction

Dynamics Dossier provides a comprehensive REST API that enables integration with external systems, custom applications, and third-party services. The API follows Microsoft Business Central's standard web service architecture and provides secure, scalable access to dossier data and functionality.

## API Architecture

### RESTful Design
- **REST Principles**: Follows REST architectural patterns with predictable URLs
- **HTTP Methods**: Standard GET, POST, PATCH, DELETE operations
- **JSON Format**: All data exchange uses JSON format
- **Stateless**: Each API call is independent and contains all necessary information

### Base URL Structure
```
https://{environment}.api.businesscentral.dynamics.com/v2.0/{tenant}/Production/ODataV4/Company('{company}')/
```

**Parameters**:
- `{environment}`: Your Business Central environment (e.g., `mycompany`)
- `{tenant}`: Azure AD tenant ID
- `{company}`: Company name in Business Central

## Authentication

### OAuth 2.0 Authentication
All API requests require OAuth 2.0 authentication:

1. **Register Application**: Register your application in Azure AD
2. **Request Token**: Obtain access token using client credentials or authorization code flow
3. **Include Token**: Include bearer token in all API requests

### Authentication Header
```http
Authorization: Bearer {access_token}
```

### Required Permissions
- **Business Central Access**: Basic Business Central API access
- **Dynamics Dossier**: Specific permissions for dossier operations
- **Company Access**: Permissions for the specific company in Business Central

## Core API Endpoints

### Dossiers

#### Get All Dossiers
```http
GET /api/v2.0/dossiers
```

**Response Example**:
```json
{
  "value": [
    {
      "id": "11195787",
      "no": "D0001",
      "description": "Employee Medical Assessment",
      "primaryContactNo": "C001",
      "dossierTypeCode": "MEDICAL",
      "status": "Open",
      "documentDate": "2026-03-01",
      "startingDate": "2026-03-01",
      "endingDate": "2026-06-01"
    }
  ]
}
```

#### Get Specific Dossier
```http
GET /api/v2.0/dossiers('{dossier-id}')
```

#### Create New Dossier
```http
POST /api/v2.0/dossiers
Content-Type: application/json

{
  "primaryContactNo": "C001",
  "description": "New Medical Assessment",
  "dossierTypeCode": "MEDICAL",
  "documentDate": "2026-03-07",
  "startingDate": "2026-03-07"
}
```

#### Update Dossier
```http
PATCH /api/v2.0/dossiers('{dossier-id}')
Content-Type: application/json

{
  "status": "In Process",
  "endingDate": "2026-04-01"
}
```

#### Delete Dossier
```http
DELETE /api/v2.0/dossiers('{dossier-id}')
```

### Dossier Types

#### Get All Dossier Types
```http
GET /api/v2.0/dossierTypes
```

**Response Example**:
```json
{
  "value": [
    {
      "code": "MEDICAL",
      "description": "Medical Records",
      "category": "Medical",
      "sickness": false,
      "durationOfDossier": "P1Y"
    }
  ]
}
```

### Dossier Content

#### Get Dossier Documents
```http
GET /api/v2.0/dossiers('{dossier-id}')/documents
```

#### Upload Document
```http
POST /api/v2.0/dossiers('{dossier-id}')/documents
Content-Type: multipart/form-data

{
  "description": "Medical Certificate",
  "documentType": "Medical",
  "file": [binary data]
}
```

### Workflow Management

#### Get Workflow Log
```http
GET /api/v2.0/dossiers('{dossier-id}')/workflowLog
```

#### Trigger Workflow Step
```http
POST /api/v2.0/dossiers('{dossier-id}')/workflowActions
Content-Type: application/json

{
  "action": "complete",
  "workflowLineNo": 10000,
  "comments": "Medical review completed"
}
```

### Contacts Integration

#### Get Contact Dossiers
```http
GET /api/v2.0/contacts('{contact-id}')/dossiers
```

#### Create Dossier for Contact
```http
POST /api/v2.0/contacts('{contact-id}')/dossiers
Content-Type: application/json

{
  "description": "New absence case",
  "dossierTypeCode": "ABSENCE",
  "documentDate": "2026-03-07"
}
```

## Advanced API Features

### Filtering and Querying

#### OData Query Options
Use standard OData query parameters:

```http
GET /api/v2.0/dossiers?$filter=status eq 'Open'&$orderby=documentDate desc&$top=50
```

#### Common Filters
- **By Status**: `$filter=status eq 'Open'`
- **By Type**: `$filter=dossierTypeCode eq 'MEDICAL'`
- **By Date Range**: `$filter=documentDate ge 2026-01-01 and documentDate le 2026-12-31`
- **By Contact**: `$filter=primaryContactNo eq 'C001'`

#### Select Specific Fields
```http
GET /api/v2.0/dossiers?$select=no,description,status,documentDate
```

### Batch Operations

#### Batch Requests
Combine multiple operations in a single HTTP request:

```http
POST /api/v2.0/$batch
Content-Type: multipart/mixed; boundary=batch_123

--batch_123
Content-Type: application/http
Content-Transfer-Encoding: binary

GET /api/v2.0/dossiers HTTP/1.1

--batch_123
Content-Type: application/http
Content-Transfer-Encoding: binary

POST /api/v2.0/dossiers HTTP/1.1
Content-Type: application/json

{
  "primaryContactNo": "C002",
  "description": "Batch Created Dossier"
}

--batch_123--
```

## Error Handling

### HTTP Status Codes

| Code | Description | Action |
|------|-------------|--------|
| 200 | Success | Request processed successfully |
| 201 | Created | Resource created successfully |
| 400 | Bad Request | Check request format and parameters |
| 401 | Unauthorized | Verify authentication token |
| 403 | Forbidden | Check user permissions |
| 404 | Not Found | Verify resource ID exists |
| 500 | Server Error | Contact support if persistent |

### Error Response Format
```json
{
  "error": {
    "code": "InvalidRequest",
    "message": "The dossier type 'INVALID' does not exist.",
    "details": [
      {
        "code": "ValidationError",
        "message": "DossierTypeCode field validation failed",
        "target": "dossierTypeCode"
      }
    ]
  }
}
```

## Rate Limiting

### Request Limits
- **Requests per minute**: 100 requests per user per minute
- **Burst limit**: 20 requests per second
- **Daily limit**: 10,000 requests per user per day

### Headers
Response headers include rate limit information:
```http
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 95
X-RateLimit-Reset: 1646676000
```

## Webhooks and Events

### Event Subscription
Subscribe to dossier events for real-time notifications:

```http
POST /api/v2.0/subscriptions
Content-Type: application/json

{
  "resource": "dossiers",
  "changeType": "created,updated",
  "notificationUrl": "https://your-app.com/webhooks/dossiers",
  "expirationDateTime": "2026-04-01T00:00:00Z"
}
```

### Event Types
- **dossier.created**: New dossier created
- **dossier.updated**: Dossier information changed  
- **dossier.status.changed**: Status transition occurred
- **workflow.step.completed**: Workflow step finished
- **document.added**: Document uploaded to dossier

### Webhook Payload Example
```json
{
  "subscriptionId": "sub-123",
  "changeType": "created",
  "resource": "dossiers",
  "resourceData": {
    "id": "D0123",
    "no": "D0123",
    "primaryContactNo": "C001",
    "status": "Open"
  },
  "timestamp": "2026-03-07T10:30:00Z"
}
```

## SDK and Code Examples

### C# Example
```csharp
using Microsoft.Graph;
using System.Net.Http.Headers;

public class DossierApiClient
{
    private readonly HttpClient _httpClient;
    
    public DossierApiClient(string accessToken)
    {
        _httpClient = new HttpClient();
        _httpClient.DefaultRequestHeaders.Authorization = 
            new AuthenticationHeaderValue("Bearer", accessToken);
    }
    
    public async Task<Dossier> CreateDossierAsync(CreateDossierRequest request)
    {
        var json = JsonSerializer.Serialize(request);
        var content = new StringContent(json, Encoding.UTF8, "application/json");
        
        var response = await _httpClient.PostAsync(
            "https://mycompany.api.businesscentral.dynamics.com/v2.0/tenant/Production/ODataV4/Company('CRONUS')/api/v2.0/dossiers",
            content);
            
        response.EnsureSuccessStatusCode();
        var responseJson = await response.Content.ReadAsStringAsync();
        return JsonSerializer.Deserialize<Dossier>(responseJson);
    }
}
```

### JavaScript Example
```javascript
class DossierApi {
    constructor(accessToken, baseUrl) {
        this.accessToken = accessToken;
        this.baseUrl = baseUrl;
    }
    
    async getDossiers(filters = {}) {
        const queryParams = new URLSearchParams(filters);
        const response = await fetch(`${this.baseUrl}/dossiers?${queryParams}`, {
            headers: {
                'Authorization': `Bearer ${this.accessToken}`,
                'Content-Type': 'application/json'
            }
        });
        
        if (!response.ok) {
            throw new Error(`HTTP ${response.status}: ${response.statusText}`);
        }
        
        return await response.json();
    }
    
    async createDossier(dossierData) {
        const response = await fetch(`${this.baseUrl}/dossiers`, {
            method: 'POST',
            headers: {
                'Authorization': `Bearer ${this.accessToken}`,
                'Content-Type': 'application/json'
            },
            body: JSON.stringify(dossierData)
        });
        
        return await response.json();
    }
}
```

### Python Example
```python
import requests
import json

class DossierApi:
    def __init__(self, access_token, base_url):
        self.access_token = access_token
        self.base_url = base_url
        self.headers = {
            'Authorization': f'Bearer {access_token}',
            'Content-Type': 'application/json'
        }
    
    def get_dossiers(self, filters=None):
        url = f"{self.base_url}/dossiers"
        params = filters if filters else {}
        
        response = requests.get(url, headers=self.headers, params=params)
        response.raise_for_status()
        return response.json()
    
    def create_dossier(self, dossier_data):
        url = f"{self.base_url}/dossiers"
        
        response = requests.post(
            url, 
            headers=self.headers, 
            data=json.dumps(dossier_data)
        )
        response.raise_for_status()
        return response.json()
```

## Data Models

### Dossier Object
```json
{
  "id": "string",
  "no": "string",
  "description": "string",
  "primaryContactNo": "string",
  "dossierTypeCode": "string",
  "status": "enum[New,Open,InProcess,Closed]",
  "documentDate": "date",
  "startingDate": "date", 
  "endingDate": "date",
  "dateClosed": "date",
  "externalDocumentNo": "string",
  "yourReference": "string",
  "ourReference": "string",
  "priority": "enum[Low,Normal,High]",
  "confidentialityLevel": "integer",
  "category": "enum[Absence,Medical,Reintegration,Examination,Safety]"
}
```

### DossierType Object
```json
{
  "code": "string",
  "description": "string",
  "sickness": "boolean",
  "durationOfDossier": "duration",
  "category": "enum[Absence,Medical,Reintegration,Examination,Safety]"
}
```

## Best Practices

### API Usage Guidelines
✅ **Authentication**: Always secure API keys and refresh tokens appropriately  
✅ **Rate Limiting**: Implement proper rate limiting and retry logic  
✅ **Error Handling**: Handle all possible HTTP status codes gracefully  
✅ **Data Validation**: Validate data before sending API requests  

### Performance Optimization
✅ **Batch Requests**: Use batch operations for multiple related requests  
✅ **Selective Fields**: Use `$select` to retrieve only needed fields  
✅ **Caching**: Cache frequently accessed data appropriately  
✅ **Pagination**: Use `$top` and `$skip` for large result sets  

### Security Considerations  
✅ **HTTPS Only**: Always use HTTPS for API communications  
✅ **Token Management**: Securely store and rotate access tokens  
✅ **Input Validation**: Validate and sanitize all input data  
✅ **Audit Logging**: Log all API access for security monitoring  

## Support and Resources

### API Documentation
- **Interactive API Explorer**: Available in Business Central admin center
- **OpenAPI Specification**: Download complete API specification
- **Sample Requests**: Postman collection available for download

### Developer Support
- **Technical Support**: [support@q-team.nl](mailto:support@q-team.nl)  
- **Developer Forum**: Community support and discussions
- **Integration Services**: Professional integration assistance available

### Related Documentation
- [Authentication Guide](authentication.md)
- [Extensibility Guide](extensibility-guide.md)
- [Troubleshooting](../troubleshooting/common-errors.md)

---

*Next: Learn about [Authentication](authentication.md) setup and configuration for API access.*