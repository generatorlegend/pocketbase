---
title: File Uploads
---

# File Uploads

PocketBase provides a robust system for handling file uploads, allowing you to easily associate files with records and manage them efficiently. This guide will walk you through the process of configuring file uploads, understanding storage options, and working with files in your PocketBase application.

## Configuration

PocketBase supports two main storage options for file uploads:

1. Local filesystem
2. S3-compatible object storage

### Local Filesystem

To use local filesystem storage, you can initialize it using the `NewLocal` function:

```go
fs, err := filesystem.NewLocal("./pb_data/storage")
if err != nil {
    // Handle error
}
defer fs.Close()
```

This will store uploaded files in the specified directory on your local machine.

### S3-compatible Object Storage

For S3-compatible storage, use the `NewS3` function:

```go
fs, err := filesystem.NewS3(
    "your-bucket-name",
    "your-region",
    "your-endpoint",
    "your-access-key",
    "your-secret-key",
    false // Set to true if using path-style S3 URLs
)
if err != nil {
    // Handle error
}
defer fs.Close()
```

This allows you to store files in services like Amazon S3, MinIO, or any other S3-compatible object storage.

## Associating Files with Records

To associate files with records, you'll need to create a field of type "File" in your collection schema. This can be done through the Admin UI or programmatically.

When users upload files through your application, PocketBase will handle the storage and association with the corresponding record automatically.

## Uploading Files

### Backend (Go)

To upload files from your Go backend, you can use the `UploadFile` method:

```go
file := &filesystem.File{
    Name: "example.txt",
    // Other file properties...
}

err := fs.UploadFile(file, "path/to/store/example.txt")
if err != nil {
    // Handle error
}
```

### Frontend (JavaScript)

For frontend file uploads, you can use the PocketBase SDK or make direct API calls. Here's an example using the SDK:

```javascript
const formData = new FormData();
formData.append('file', fileInput.files[0]);

await pb.collection('your_collection').create(formData);
```

## Retrieving Files

To retrieve files associated with a record, you can use the `GetFile` method:

```go
file, err := fs.GetFile("path/to/file.txt")
if err != nil {
    // Handle error
}
defer file.Close()

// Use the file...
```

## Serving Files

PocketBase provides a convenient way to serve files using the `Serve` method:

```go
http.HandleFunc("/files/", func(w http.ResponseWriter, r *http.Request) {
    fileKey := r.URL.Path[len("/files/"):]
    err := fs.Serve(w, r, fileKey, "filename.ext")
    if err != nil {
        // Handle error
    }
})
```

This method handles range requests and sets appropriate headers for efficient file serving.

## Thumbnail Generation

PocketBase supports automatic thumbnail generation for image files. You can create thumbnails using the `CreateThumb` method:

```go
err := fs.CreateThumb("original/image.jpg", "thumb/image_thumb.jpg", "100x100")
if err != nil {
    // Handle error
}
```

The thumb size can be specified in various formats:
- `0xH`: Resize to H height preserving the aspect ratio
- `Wx0`: Resize to W width preserving the aspect ratio
- `WxH`: Resize and crop to WxH viewbox (from center)
- `WxHt`: Resize and crop to WxH viewbox (from top)
- `WxHb`: Resize and crop to WxH viewbox (from bottom)
- `WxHf`: Fit inside a WxH viewbox (without cropping)

## File Management

PocketBase provides several methods for managing files:

- `Exists(fileKey string)`: Check if a file exists
- `Attributes(fileKey string)`: Get file attributes
- `Delete(fileKey string)`: Delete a file
- `DeletePrefix(prefix string)`: Delete multiple files with a common prefix

## Best Practices

1. Always validate file types and sizes before uploading to prevent security issues.
2. Use appropriate file permissions and access controls to protect sensitive files.
3. Implement error handling for all file operations.
4. Consider using a CDN for improved file serving performance, especially for larger files or high-traffic applications.
5. Regularly backup your file storage, whether using local filesystem or S3-compatible storage.

By following these guidelines and utilizing PocketBase's file upload features, you can efficiently manage file uploads and storage in your application while ensuring good performance and security.