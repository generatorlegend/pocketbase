# Security Best Practices

This guide outlines security best practices for using PocketBase in production environments. Following these recommendations will help you enhance the security of your PocketBase applications.

## Authentication

### Use Strong Authentication Methods

1. Enable email verification for new user registrations to prevent unauthorized account creation.
2. Implement Multi-Factor Authentication (MFA) for sensitive operations or high-privilege accounts.
3. Use OAuth2 providers for third-party authentication when possible.

Example of enabling email verification:

```go
app.OnRecordBeforeCreateRequest().Add(func(e *core.RecordCreateEvent) error {
    if e.Collection.Name == "users" {
        e.Record.Set("verified", false)
    }
    return nil
})
```

### Secure Password Handling

1. Enforce strong password policies (e.g., minimum length, complexity requirements).
2. Use PocketBase's built-in password hashing and avoid storing plain-text passwords.

## Data Access Controls

### Implement Proper Authorization

1. Use PocketBase's built-in roles and permissions system to control access to collections and records.
2. Apply the principle of least privilege: grant only the minimum necessary permissions to users and API keys.

Example of setting up role-based access control:

```go
app.OnRecordBeforeCreateRequest().Add(func(e *core.RecordCreateEvent) error {
    if e.Collection.Name == "sensitive_data" {
        if !e.HttpContext.IsAdmin() {
            return errors.New("Only admins can create records in this collection")
        }
    }
    return nil
})
```

### Secure API Endpoints

1. Use HTTPS for all communications to encrypt data in transit.
2. Implement rate limiting to prevent abuse and DoS attacks.
3. Validate and sanitize all user inputs to prevent injection attacks.

PocketBase provides built-in rate limiting. You can customize it like this:

```go
app.OnBeforeServe().Add(func(e *core.ServeEvent) error {
    e.Router.AddRoute(echo.Route{
        Method: http.MethodGet,
        Path:   "/custom/endpoint",
        Handler: apis.RequireAdminOrUserAuth(
            apis.RequireRequestPatternRateLimit(
                "1-10/m",
                func(c echo.Context) error {
                    // Your custom endpoint logic here
                    return c.String(200, "Hello!")
                },
            ),
        ),
    })
    return nil
})
```

## File Security

### Secure File Uploads

1. Limit file types and sizes to prevent malicious uploads.
2. Scan uploaded files for malware if possible.
3. Store uploaded files in a secure location, preferably outside the web root.

PocketBase uses a separate storage directory for file uploads by default, enhancing security.

### Protect Sensitive Files

1. Use PocketBase's file token system to control access to uploaded files.
2. Implement proper access controls for admin files and backups.

## API Security

### Secure API Keys

1. Rotate API keys regularly.
2. Use separate API keys for different environments (development, staging, production).
3. Implement proper access controls and monitoring for API key usage.

### Implement CORS Properly

Configure CORS (Cross-Origin Resource Sharing) to restrict access to your API from unauthorized domains:

```go
app.OnBeforeServe().Add(func(e *core.ServeEvent) error {
    e.Router.Use(middleware.CORSWithConfig(middleware.CORSConfig{
        AllowOrigins: []string{"https://yourdomain.com"},
        AllowMethods: []string{http.MethodGet, http.MethodPost, http.MethodPut, http.MethodDelete},
    }))
    return nil
})
```

## Logging and Monitoring

1. Enable and regularly review PocketBase logs for suspicious activities.
2. Implement proper error handling to avoid exposing sensitive information in error messages.
3. Set up alerts for unusual activities or failed login attempts.

## Regular Updates and Maintenance

1. Keep PocketBase and all dependencies up to date with the latest security patches.
2. Regularly backup your database and test the restoration process.
3. Conduct periodic security audits of your application and infrastructure.

By following these security best practices, you can significantly enhance the security of your PocketBase applications in production environments. Remember to stay informed about the latest security threats and continuously adapt your security measures accordingly.