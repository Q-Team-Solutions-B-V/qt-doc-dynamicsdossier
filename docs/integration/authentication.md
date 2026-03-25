# Authentication

## Overview

Dynamics Dossier authentication follows Microsoft Business Central's standard security model while providing additional security features through the Q-Team App Authenticator. This guide covers authentication setup, API access, and security best practices.

## Authentication Methods

### Business Central User Authentication

#### Standard User Login
Users authenticate through Business Central's standard authentication:

1. **Azure AD Integration**: For Business Central Online
2. **Windows Authentication**: For on-premises deployments  
3. **Web Service Access Keys**: For programmatic access
4. **OAuth 2.0**: For modern API integrations

#### Q-Team App Authenticator
Additional authentication layer providing:
- **Enhanced security** for sensitive dossier data
- **License validation** and compliance checking
- **Extended session management** for long-running processes
- **Audit logging** for security compliance

## Setting Up API Authentication

### OAuth 2.0 Configuration

#### Step 1: Register Application in Azure AD
1. **Navigate to**: Azure Portal → App Registrations
2. **Create new registration**:
   ```
   Name: Dossier API Integration
   Account types: Accounts in this organizational directory only
   Redirect URI: https://your-app.com/auth/callback
   ```

3. **Configure API permissions**:
   ```
   Required Permissions:
   - Dynamics 365 Business Central (user_impersonation)
   - Microsoft Graph (offline_access, openid, profile)
   ```

#### Step 2: Generate Client Secret
1. **Go to**: Certificates & secrets → New client secret
2. **Configure**:
   ```
   Description: Dossier API Access
   Expires: 24 months (recommended)
   ```
3. **Copy secret value** (save securely immediately)

#### Step 3: Configure Business Central
1. **In Business Central**: Setup → Azure Active Directory Applications
2. **Create new entry**:
   ```
   Client ID: [From Azure AD app registration]
   Description: Dossier API Integration
   State: Enabled
   ```

### Service-to-Service Authentication

#### Using Client Credentials Flow
For server-to-server integrations without user interaction:

```http
POST https://login.microsoftonline.com/{tenant-id}/oauth2/v2.0/token
Content-Type: application/x-www-form-urlencoded

client_id={client-id}
&scope=https://api.businesscentral.dynamics.com/.default
&client_secret={client-secret}
&grant_type=client_credentials
```

#### Code Example (C#)
```csharp
public class AuthenticationService
{
    private readonly string _tenantId;
    private readonly string _clientId;
    private readonly string _clientSecret;
    
    public async Task<string> GetAccessTokenAsync()
    {
        var client = new HttpClient();
        var tokenEndpoint = $"https://login.microsoftonline.com/{_tenantId}/oauth2/v2.0/token";
        
        var parameters = new Dictionary<string, string>
        {
            {"client_id", _clientId},
            {"client_secret", _clientSecret},
            {"scope", "https://api.businesscentral.dynamics.com/.default"},
            {"grant_type", "client_credentials"}
        };
        
        var content = new FormUrlEncodedContent(parameters);
        var response = await client.PostAsync(tokenEndpoint, content);
        var responseString = await response.Content.ReadAsStringAsync();
        
        var tokenResponse = JsonSerializer.Deserialize<TokenResponse>(responseString);
        return tokenResponse.AccessToken;
    }
}
```

### User Authentication Flow

#### Authorization Code Flow
For applications requiring user interaction:

#### Step 1: Authorization Request
```http
GET https://login.microsoftonline.com/{tenant-id}/oauth2/v2.0/authorize
?client_id={client-id}
&response_type=code
&redirect_uri={redirect-uri}
&scope=https://api.businesscentral.dynamics.com/user_impersonation offline_access
&state={state-value}
```

#### Step 2: Exchange Code for Token
```http
POST https://login.microsoftonline.com/{tenant-id}/oauth2/v2.0/token
Content-Type: application/x-www-form-urlencoded

client_id={client-id}
&scope=https://api.businesscentral.dynamics.com/user_impersonation
&code={authorization-code}
&redirect_uri={redirect-uri}
&grant_type=authorization_code
&client_secret={client-secret}
```

#### JavaScript Example
```javascript
class DossierAuth {
    constructor(clientId, tenantId, redirectUri) {
        this.clientId = clientId;
        this.tenantId = tenantId;
        this.redirectUri = redirectUri;
    }
    
    getAuthorizationUrl() {
        const authUrl = `https://login.microsoftonline.com/${this.tenantId}/oauth2/v2.0/authorize`;
        const params = new URLSearchParams({
            client_id: this.clientId,
            response_type: 'code',
            redirect_uri: this.redirectUri,
            scope: 'https://api.businesscentral.dynamics.com/user_impersonation offline_access',
            state: this.generateState()
        });
        
        return `${authUrl}?${params.toString()}`;
    }
    
    async exchangeCodeForToken(authorizationCode) {
        const tokenEndpoint = `https://login.microsoftonline.com/${this.tenantId}/oauth2/v2.0/token`;
        
        const params = new URLSearchParams({
            client_id: this.clientId,
            scope: 'https://api.businesscentral.dynamics.com/user_impersonation',
            code: authorizationCode,
            redirect_uri: this.redirectUri,
            grant_type: 'authorization_code',
            client_secret: this.clientSecret
        });
        
        const response = await fetch(tokenEndpoint, {
            method: 'POST',
            headers: { 'Content-Type': 'application/x-www-form-urlencoded' },
            body: params
        });
        
        return await response.json();
    }
}
```

## API Authentication Implementation

### Making Authenticated API Calls

#### Request Headers
Include authentication token in all API requests:

```http
GET /api/v2.0/dossiers
Host: {environment}.api.businesscentral.dynamics.com
Authorization: Bearer {access_token}
Content-Type: application/json
Accept: application/json
```

#### Token Refresh
Implement automatic token refresh for long-running applications:

```csharp
public class TokenManager
{
    private string _accessToken;
    private string _refreshToken;
    private DateTime _tokenExpiry;
    
    public async Task<string> GetValidTokenAsync()
    {
        if (DateTime.UtcNow >= _tokenExpiry.AddMinutes(-5))
        {
            await RefreshTokenAsync();
        }
        
        return _accessToken;
    }
    
    private async Task RefreshTokenAsync()
    {
        var client = new HttpClient();
        var tokenEndpoint = $"https://login.microsoftonline.com/{_tenantId}/oauth2/v2.0/token";
        
        var parameters = new Dictionary<string, string>
        {
            {"client_id", _clientId},
            {"client_secret", _clientSecret},
            {"refresh_token", _refreshToken},
            {"grant_type", "refresh_token"}
        };
        
        var content = new FormUrlEncodedContent(parameters);
        var response = await client.PostAsync(tokenEndpoint, content);
        var tokenResponse = await response.Content.ReadFromJsonAsync<TokenResponse>();
        
        _accessToken = tokenResponse.AccessToken;
        _refreshToken = tokenResponse.RefreshToken ?? _refreshToken;
        _tokenExpiry = DateTime.UtcNow.AddSeconds(tokenResponse.ExpiresIn);
    }
}
```

## Web Service Access Keys (Legacy)

### Creating Web Service Access Keys
For compatibility with older integrations:

1. **In Business Central**: My Settings → Web Service Access Keys
2. **Generate new key**:
   ```
   Name: Dossier API Integration
   Expiry Date: [Set appropriate date]
   ```
3. **Copy the generated key** (displayed only once)

### Using Access Keys
Include in HTTP Basic authentication:

```http
GET /api/v2.0/dossiers
Host: {environment}.api.businesscentral.dynamics.com
Authorization: Basic {base64(username:accesskey)}
```

## Security Configuration

### Role-Based Access Control

#### Permission Sets for API Access
Create specific permission sets for API users:

```
DOSSIER API - READ:
- Read access to dossier tables
- Read access to contact information
- No modification permissions

DOSSIER API - WRITE:
- Read and write access to dossiers
- Document upload permissions
- Workflow execution rights

DOSSIER API - ADMIN:
- Full administrative access
- System configuration rights
- User management permissions
```

#### Granular Permissions
Configure field-level security for sensitive data:

| Field Category | API Read | API Write | Special Permissions |
|---------------|----------|-----------|-------------------|
| Basic Info | ✅ | ✅ | Standard access |
| Medical Data | ⚠️ | ⚠️ | Medical role required |
| Financial | ⚠️ | ❌ | Finance role required |
| Confidential | ❌ | ❌ | Admin approval required |

### Security Best Practices

#### Token Security
- **Secure Storage**: Store tokens in secure, encrypted storage
- **Short Expiry**: Use short-lived tokens with refresh capability
- **Scope Limitation**: Request minimal required scopes
- **Rotation**: Regularly rotate client secrets and keys

#### Network Security
- **HTTPS Only**: Always use encrypted connections
- **IP Whitelisting**: Restrict API access to known IP addresses
- **Rate Limiting**: Implement client-side rate limiting
- **Monitoring**: Log all API access for security monitoring

#### Data Protection
- **Minimal Data**: Request only necessary data through APIs
- **Data Retention**: Implement proper data retention policies
- **Audit Logging**: Maintain comprehensive audit logs
- **Compliance**: Ensure GDPR and regulatory compliance

## Troubleshooting Authentication

### Common Authentication Errors

#### "Invalid Client" Error
**Problem**: Client ID or secret is incorrect
**Solutions**:
- Verify client ID matches Azure AD registration
- Check client secret hasn't expired
- Confirm application is registered in correct tenant

#### "Insufficient Permissions" Error  
**Problem**: Token lacks required permissions
**Solutions**:
- Check requested scopes in token request
- Verify admin consent has been granted
- Confirm user has appropriate Business Central permissions

#### "Token Expired" Error
**Problem**: Access token has expired
**Solutions**:
- Implement automatic token refresh
- Check token expiry before making requests
- Use refresh token to obtain new access token

### Authentication Debugging

#### Logging Authentication Events
```csharp
public class AuthenticationLogger
{
    private readonly ILogger _logger;
    
    public void LogTokenRequest(string clientId, string scope)
    {
        _logger.LogInformation("Token requested for client {ClientId} with scope {Scope}", 
            clientId, scope);
    }
    
    public void LogAuthenticationFailure(string error, string description)
    {
        _logger.LogError("Authentication failed: {Error} - {Description}", 
            error, description);
    }
    
    public void LogApiCall(string endpoint, string method, int statusCode)
    {
        _logger.LogInformation("API call: {Method} {Endpoint} returned {StatusCode}", 
            method, endpoint, statusCode);
    }
}
```

#### Token Validation Test
```csharp
public async Task<bool> ValidateTokenAsync(string token)
{
    try
    {
        var client = new HttpClient();
        client.DefaultRequestHeaders.Authorization = 
            new AuthenticationHeaderValue("Bearer", token);
            
        var response = await client.GetAsync(
            "https://api.businesscentral.dynamics.com/v2.0/me");
            
        return response.IsSuccessStatusCode;
    }
    catch
    {
        return false;
    }
}
```

## Advanced Authentication Scenarios

### Multi-Tenant Applications
For applications serving multiple Business Central tenants:

```csharp
public class MultiTenantAuthService
{
    public async Task<string> GetTenantTokenAsync(string tenantId)
    {
        var tokenEndpoint = $"https://login.microsoftonline.com/{tenantId}/oauth2/v2.0/token";
        
        // Implementation specific to tenant
        return await RequestTokenAsync(tokenEndpoint);
    }
    
    public string GetTenantApiUrl(string tenantId, string environment)
    {
        return $"https://{environment}.api.businesscentral.dynamics.com/v2.0/{tenantId}/Production/ODataV4/";
    }
}
```

### Certificate-Based Authentication
For enhanced security in enterprise environments:

```csharp
public class CertificateAuthService
{
    private readonly X509Certificate2 _certificate;
    
    public async Task<string> GetCertificateTokenAsync()
    {
        var assertion = CreateJwtAssertion();
        
        var parameters = new Dictionary<string, string>
        {
            {"client_id", _clientId},
            {"client_assertion_type", "urn:ietf:params:oauth:client-assertion-type:jwt-bearer"},
            {"client_assertion", assertion},
            {"scope", "https://api.businesscentral.dynamics.com/.default"},
            {"grant_type", "client_credentials"}
        };
        
        // Token request with certificate assertion
        return await RequestTokenAsync(parameters);
    }
}
```

## Support and Resources

### Getting Help with Authentication
- **Documentation**: [Microsoft Identity Platform](https://docs.microsoft.com/en-us/azure/active-directory/develop/)
- **Q-Team Support**: [support@q-team.nl](mailto:support@q-team.nl)
- **Business Central APIs**: [BC API Documentation](https://docs.microsoft.com/en-us/dynamics365/business-central/dev-itpro/api-reference/v2.0/)

### Related Documentation
- [API Overview](api-overview.md)
- [Extensibility Guide](extensibility-guide.md)
- [Common Errors](../troubleshooting/common-errors.md)

---

*Next: Learn about [Extensibility](extensibility-guide.md) options for customizing Dynamics Dossier.*