---
title: Record CRUD Operations
---

# Record CRUD Operations

This guide explains how to perform CRUD (Create, Read, Update, Delete) operations on records in PocketBase, both through the admin UI and programmatically via the API.

## Admin UI

The PocketBase Admin UI provides a user-friendly interface for managing records in your collections.

### Create a Record

1. Navigate to the desired collection in the Admin UI.
2. Click the "New record" button.
3. Fill in the required fields and any optional fields.
4. Click "Create" to save the new record.

### Read Records

1. Navigate to the desired collection in the Admin UI.
2. You'll see a list of existing records.
3. Use the search bar or filters to find specific records.
4. Click on a record to view its details.

### Update a Record

1. Navigate to the desired collection in the Admin UI.
2. Click on the record you want to update.
3. Modify the fields as needed.
4. Click "Save changes" to update the record.

### Delete a Record

1. Navigate to the desired collection in the Admin UI.
2. Click on the record you want to delete.
3. Click the "Delete" button in the top-right corner.
4. Confirm the deletion in the popup dialog.

## API

PocketBase provides a RESTful API for programmatically performing CRUD operations on records.

### Create a Record

To create a new record, send a POST request to the `/api/collections/:collectionName/records` endpoint:

```javascript
const newRecord = await pb.collection('example').create({
  field1: 'value1',
  field2: 'value2',
});
```

### Read Records

To retrieve records, use the GET method:

- List records: `/api/collections/:collectionName/records`
- Get a single record: `/api/collections/:collectionName/records/:id`

```javascript
// List records
const records = await pb.collection('example').getList(1, 20, {
  filter: 'created >= "2022-01-01 00:00:00"',
});

// Get a single record
const record = await pb.collection('example').getOne('RECORD_ID');
```

### Update a Record

To update an existing record, send a PATCH request to `/api/collections/:collectionName/records/:id`:

```javascript
const updatedRecord = await pb.collection('example').update('RECORD_ID', {
  field1: 'new value',
});
```

### Delete a Record

To delete a record, send a DELETE request to `/api/collections/:collectionName/records/:id`:

```javascript
await pb.collection('example').delete('RECORD_ID');
```

## Access Rules

PocketBase uses rule-based permissions to control access to records. Make sure to configure the appropriate rules for your collections:

- `listRule`: Determines who can list and search records
- `viewRule`: Controls who can view individual records
- `createRule`: Specifies who can create new records
- `updateRule`: Defines who can update existing records
- `deleteRule`: Sets who can delete records

These rules can be configured in the Admin UI under the collection's Settings tab.

## File Uploads

For fields of type `file`, you can handle file uploads during record creation or updates:

```javascript
const formData = new FormData();
formData.append('field1', 'value1');
formData.append('file', fileInput.files[0]);

const createdRecord = await pb.collection('example').create(formData);
```

## Batch Operations

PocketBase also supports batch operations for creating, updating, and deleting multiple records at once. Refer to the API documentation for more details on batch endpoints.

## Error Handling

When performing CRUD operations, be sure to implement proper error handling:

```javascript
try {
  const record = await pb.collection('example').create({
    field1: 'value1',
  });
  console.log('Record created:', record);
} catch (error) {
  console.error('Error creating record:', error);
}
```

By following these guidelines, you can effectively manage records in your PocketBase application using both the Admin UI and the API.