# PocketBase Admin UI Guide

This guide provides a comprehensive overview of the PocketBase Admin UI, including collection management, record editing, user management, and system configuration.

## Table of Contents

1. [Overview](#overview)
2. [Collections](#collections)
   - [Creating a Collection](#creating-a-collection)
   - [Editing a Collection](#editing-a-collection)
   - [Collection Types](#collection-types)
   - [Field Types](#field-types)
3. [Records](#records)
   - [Creating a Record](#creating-a-record)
   - [Editing a Record](#editing-a-record)
   - [Deleting a Record](#deleting-a-record)
4. [User Management](#user-management)
   - [Auth Collections](#auth-collections)
   - [User Authentication](#user-authentication)
5. [System Configuration](#system-configuration)
6. [API Rules](#api-rules)

## Overview

The PocketBase Admin UI is a powerful interface for managing your PocketBase application. It provides an intuitive way to handle collections, records, users, and system settings.

## Collections

Collections are the core of your PocketBase application. They represent tables in your database and define the structure of your data.

### Creating a Collection

To create a new collection:

1. Click on the "Collections" icon in the sidebar.
2. Click the "New collection" button.
3. Choose a collection type (Base, Auth, or View).
4. Enter a name for your collection.
5. Add fields to define the structure of your collection.
6. Configure API rules and other settings as needed.
7. Click "Create" to save the new collection.

### Editing a Collection

To edit an existing collection:

1. Click on the collection in the Collections list.
2. Use the tabs (Schema, API Rules, Indexes, etc.) to modify different aspects of the collection.
3. Make your changes and click "Save changes" to update the collection.

### Collection Types

PocketBase supports three types of collections:

- **Base**: Standard collections for storing general data.
- **Auth**: Special collections for user authentication and management.
- **View**: Collections that represent database views, allowing you to create custom data representations.

### Field Types

PocketBase supports various field types, including:

- Text
- Number
- Boolean
- Email
- URL
- Date
- Select
- Relation
- File
- JSON
- Rich Text Editor

Each field type has its own configuration options and validation rules.

## Records

Records are individual entries in your collections. The Admin UI provides an intuitive interface for managing records.

### Creating a Record

To create a new record:

1. Navigate to the desired collection.
2. Click the "New record" button.
3. Fill in the fields with the appropriate data.
4. Click "Create" to save the new record.

### Editing a Record

To edit an existing record:

1. Click on the record in the list view.
2. Modify the fields as needed.
3. Click "Save changes" to update the record.

### Deleting a Record

To delete a record:

1. Open the record you want to delete.
2. Click the "More options" (three dots) button.
3. Select "Delete" from the dropdown menu.
4. Confirm the deletion when prompted.

## User Management

PocketBase provides built-in user management capabilities through Auth collections.

### Auth Collections

Auth collections are special collections designed for user management. They include additional fields and functionality for handling user authentication and authorization.

To create an Auth collection:

1. Follow the steps for creating a new collection.
2. Choose "Auth" as the collection type.
3. Configure additional options such as authentication options and rules.

### User Authentication

The Admin UI allows you to manage user accounts, including:

- Viewing user details
- Resetting passwords
- Verifying email addresses
- Managing OAuth providers

To access these features, navigate to your Auth collection and select a user record.

## System Configuration

The Admin UI provides access to various system settings and configurations. To access these settings:

1. Click on the "Settings" icon in the sidebar.
2. Use the available tabs to configure different aspects of your PocketBase application, such as:
   - General settings
   - API settings
   - SMTP configuration
   - Storage providers
   - Admin accounts

## API Rules

API Rules allow you to control access to your collections and records. To configure API rules:

1. Navigate to the desired collection.
2. Click on the "API Rules" tab.
3. Set up rules for different operations (List, View, Create, Update, Delete).
4. Use the PocketBase Rule syntax to define conditions for access.

Example rule:
```
@request.auth.id != "" && @request.auth.id = id
```

This rule allows users to access only their own records.

Remember to save your changes after modifying API rules.