---
title: Realtime Subscriptions
---

# Realtime Subscriptions

PocketBase offers a powerful realtime subscription system that allows you to receive live updates for your collections. This guide will walk you through setting up and using realtime subscriptions in your client applications.

## Overview

The realtime system in PocketBase uses Server-Sent Events (SSE) to push updates to clients. Clients can subscribe to specific collections, records, or custom topics to receive real-time notifications when data changes.

## Connecting to the Realtime API

To establish a realtime connection, you need to connect to the `/api/realtime` endpoint. Here's an example using JavaScript:

```javascript
const eventSource = new EventSource('http://localhost:8090/api/realtime');

eventSource.onmessage = (event) => {
  const data = JSON.parse(event.data);
  console.log('Received realtime event:', data);
};

eventSource.onerror = (error) => {
  console.error('EventSource failed:', error);
};
```

## Subscribing to Topics

Once connected, you can subscribe to specific topics using the `/api/realtime` endpoint with a POST request. The subscription topics follow this format:

- `{collectionName}/{recordId}` - for a specific record
- `{collectionName}/*` - for all records in a collection

Here's an example of how to subscribe to topics:

```javascript
const subscribeToTopics = async () => {
  const response = await fetch('http://localhost:8090/api/realtime', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify({
      clientId: 'YOUR_CLIENT_ID', // Received from the initial connection
      subscriptions: [
        'users/*',
        'posts/SPECIFIC_POST_ID',
      ],
    }),
  });

  if (!response.ok) {
    throw new Error('Failed to subscribe to topics');
  }
};
```

## Handling Realtime Events

When subscribed, you'll receive events for create, update, and delete operations on the subscribed topics. The event data will contain the following information:

- `action`: The type of change (create, update, or delete)
- `record`: The affected record data

Here's an example of how to handle these events:

```javascript
eventSource.onmessage = (event) => {
  const { action, record } = JSON.parse(event.data);

  switch (action) {
    case 'create':
      console.log('New record created:', record);
      break;
    case 'update':
      console.log('Record updated:', record);
      break;
    case 'delete':
      console.log('Record deleted:', record);
      break;
  }
};
```

## Authentication and Access Control

Realtime subscriptions respect the collection's view and list rules. Clients can only receive updates for records they have permission to access. You can include authentication tokens in the subscription request to receive updates for protected resources:

```javascript
const subscribeWithAuth = async (token) => {
  const response = await fetch('http://localhost:8090/api/realtime', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'Authorization': token,
    },
    body: JSON.stringify({
      clientId: 'YOUR_CLIENT_ID',
      subscriptions: ['protectedCollection/*'],
    }),
  });

  if (!response.ok) {
    throw new Error('Failed to subscribe to topics');
  }
};
```

## Best Practices

1. **Error Handling**: Implement proper error handling and reconnection logic for your EventSource connection.
2. **Unsubscribing**: When you no longer need updates for a topic, unsubscribe to conserve server resources.
3. **Limit Subscriptions**: Subscribe only to the topics you need to minimize unnecessary network traffic.
4. **Client-Side Filtering**: Use the `filter` query parameter in your subscription to receive only relevant updates.

## Conclusion

Realtime subscriptions in PocketBase provide a powerful way to keep your client applications in sync with your backend data. By following this guide, you can implement real-time updates in your projects, creating more responsive and interactive user experiences.

For more advanced usage and configuration options, refer to the PocketBase API documentation and the `Subscriptions` section in the server configuration.