# Authentication Guide

This guide will walk you through implementing authentication in your applications using PocketBase. We'll cover user registration, login, password reset, and OAuth2 integration.

## User Registration

To implement user registration in your application:

1. Create a new record in the `users` collection using the `POST /api/collections/users/records` endpoint.
2. Include the required fields such as `username`, `email`, and `password` in the request body.

Example request:

```javascript
const response = await fetch('https://your-pocketbase-url.com/api/collections/users/records', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    username: 'newuser',
    email: 'newuser@example.com',
    password: 'securepassword',
    passwordConfirm: 'securepassword',
  }),
});

const data = await response.json();
```

## User Login

To authenticate a user:

1. Use the `POST /api/collections/users/auth-with-password` endpoint.
2. Provide the user's `email` or `username` and `password` in the request body.

Example request:

```javascript
const response = await fetch('https://your-pocketbase-url.com/api/collections/users/auth-with-password', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    identity: 'user@example.com',
    password: 'userpassword',
  }),
});

const data = await response.json();
```

On successful authentication, you'll receive a JSON response containing the user's information and an authentication token.

## Password Reset

To implement a password reset flow:

1. Request a password reset using the `POST /api/collections/users/request-password-reset` endpoint.
2. The user will receive an email with a reset link.
3. Use the `POST /api/collections/users/confirm-password-reset` endpoint to set a new password.

Example request for initiating a password reset:

```javascript
const response = await fetch('https://your-pocketbase-url.com/api/collections/users/request-password-reset', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    email: 'user@example.com',
  }),
});
```

## OAuth2 Integration

PocketBase supports OAuth2 authentication with various providers. To implement OAuth2:

1. Configure the desired OAuth2 provider in your PocketBase admin panel.
2. Use the `GET /api/oauth2-redirect` endpoint to initiate the OAuth2 flow.
3. Handle the callback and token exchange.

Example of initiating the OAuth2 flow (e.g., for Google):

```javascript
const providerName = 'google';
const redirectUrl = 'https://your-app.com/oauth/callback';

window.location.href = `https://your-pocketbase-url.com/api/oauth2-redirect?provider=${providerName}&redirect=${encodeURIComponent(redirectUrl)}`;
```

In your OAuth callback handler, exchange the received code for an authentication token:

```javascript
const response = await fetch('https://your-pocketbase-url.com/api/collections/users/auth-with-oauth2', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    provider: 'google',
    code: receivedCode,
    codeVerifier: storedCodeVerifier, // if PKCE is used
    redirectUrl: 'https://your-app.com/oauth/callback',
  }),
});

const data = await response.json();
```

## Best Practices

1. Always use HTTPS to secure communication between your application and PocketBase.
2. Store authentication tokens securely and never expose them in client-side code.
3. Implement proper error handling for authentication failures.
4. Use the `Authorization` header with the format `Bearer YOUR_TOKEN` for authenticated requests.
5. Regularly rotate and update authentication tokens to enhance security.

By following this guide, you should be able to implement a robust authentication system in your application using PocketBase. Remember to refer to the official PocketBase documentation for the most up-to-date information and advanced features.