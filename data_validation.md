# Data Validation in PocketBase

PocketBase provides robust data validation capabilities to ensure the integrity and consistency of your data. This guide covers the built-in validators, custom validation rules, and how to implement them for collections and records.

## Built-in Validators

PocketBase includes a set of built-in validators that can be applied to collection fields. These validators are automatically enforced when creating or updating records.

### Common Validators

- **Required**: Ensures that a field value is not empty.
- **Min/Max Length**: Specifies the minimum and maximum length for string fields.
- **Min/Max**: Sets the minimum and maximum values for numeric fields.
- **Pattern**: Validates the field value against a regular expression.

### Type-specific Validators

- **Email**: Validates that the field contains a properly formatted email address.
- **URL**: Ensures that the field contains a valid URL.
- **Date**: Checks if the field contains a valid date format.
- **Number**: Verifies that the field contains a numeric value.

## Custom Validation Rules

In addition to built-in validators, PocketBase allows you to define custom validation rules using the Collection API Rules.

### API Rules

API Rules are JavaScript-like expressions that can be used to implement complex validation logic. These rules can be applied to different operations:

- **Create Rule**: Validates record creation.
- **Update Rule**: Validates record updates.
- **Delete Rule**: Validates record deletion.

Example of a custom validation rule:

```javascript
// Ensure that the "status" field is either "draft" or "published"
@request.data.status = "draft" || @request.data.status = "published"
```

## Implementing Validation

### Collection-level Validation

1. Navigate to the Collections section in the PocketBase Admin UI.
2. Select the collection you want to add validation to.
3. In the "API Rules" tab, you can define custom validation rules for different operations.

### Field-level Validation

1. In the collection editor, select the field you want to add validation to.
2. In the field options, you can configure built-in validators such as "Required," "Min/Max Length," etc.
3. For more complex validations, you can use the "Options" JSON field to define custom rules.

Example of field-level custom validation:

```json
{
  "validators": {
    "custom": "@request.data.fieldName.length > 5 && @request.data.fieldName.startsWith('PB')"
  }
}
```

## Validation in Code

When working with records programmatically, validation is automatically applied. Here's an example of how validation errors are handled:

```go
record := NewRecord(collection)
record.Set("email", "invalid-email")

if err := app.Dao().SaveRecord(record); err != nil {
    if validationErr, ok := err.(validation.Errors); ok {
        // Handle validation errors
        for field, fieldErr := range validationErr {
            fmt.Printf("Field %s error: %v\n", field, fieldErr)
        }
    } else {
        // Handle other errors
        fmt.Println("Error:", err)
    }
}
```

## Best Practices

1. Use built-in validators whenever possible for common validation scenarios.
2. Implement custom validation rules for complex business logic.
3. Combine field-level and collection-level validations for comprehensive data integrity.
4. Always handle validation errors gracefully in your application code.
5. Test your validation rules thoroughly to ensure they work as expected.

By leveraging PocketBase's data validation capabilities, you can ensure that your application's data remains consistent and reliable throughout its lifecycle.