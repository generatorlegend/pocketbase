# Getting Started with PocketBase

PocketBase is a powerful and flexible open-source backend for your next project. This guide will help you get started with PocketBase, covering installation, initial setup, and basic usage examples.

## Installation

To install PocketBase, follow these steps:

1. Download the latest PocketBase version for your operating system from the [official website](https://pocketbase.io/docs/).
2. Extract the downloaded archive to a directory of your choice.

## Initial Setup

Once you've installed PocketBase, you can start using it right away. Here's how to get your PocketBase instance up and running:

1. Open a terminal or command prompt.
2. Navigate to the directory where you extracted PocketBase.
3. Run the following command to start the PocketBase server:

```bash
./pocketbase serve
```

By default, PocketBase will start on `http://127.0.0.1:8090`. You can access the admin UI by opening this URL in your web browser.

### Custom Configuration

You can customize the PocketBase server configuration using command-line flags. Here are some common options:

- `--dir`: Specify a custom data directory (default: `./pb_data`)
- `--encryptionEnv`: Set an environment variable to use as the encryption key
- `--dev`: Enable development mode for additional logging
- `--origins`: Set allowed CORS origins (default: `*`)

Example:

```bash
./pocketbase serve --dir ./my_data --origins "https://example.com"
```

## Basic Usage

Now that your PocketBase server is running, let's explore some basic usage examples.

### Creating a Collection

1. Open the admin UI in your web browser.
2. Click on "Collections" in the sidebar.
3. Click the "New collection" button.
4. Enter a name for your collection and configure its fields.
5. Click "Create" to save the new collection.

### Adding Records

1. Select your newly created collection from the sidebar.
2. Click the "New record" button.
3. Fill in the fields with your data.
4. Click "Save" to add the record to your collection.

### Querying Records

You can use the PocketBase SDK or REST API to query records from your collections. Here's a simple example using JavaScript:

```javascript
import PocketBase from 'pocketbase';

const pb = new PocketBase('http://127.0.0.1:8090');

// Fetch all records from a collection
const records = await pb.collection('your_collection_name').getList(1, 50);

console.log(records);
```

## Next Steps

Now that you've got the basics down, you can explore more advanced features of PocketBase:

- Authentication and user management
- File uploads and storage
- Realtime subscriptions
- Custom API rules and hooks

Check out the [official PocketBase documentation](https://pocketbase.io/docs/) for more detailed information on these topics and more.