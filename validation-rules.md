**On this page:**
- Required and optional request fields with types, formats, and examples
- Validation rules for data (type, format, logical consistency, time constraints)
- Supported data formats
- Examples of errors related to invalid or incomplete data (INVALID_REQUEST, VALIDATION_ERROR)

**Not covered:**
- Authorization errors (401 UNAUTHORIZED, 403 INSUFFICIENT_SCOPE)
- Business logic, conflict handling, or booking priority rules
- Any HTTP methods, successful response codes, or actual API endpoints

# Request validation

Request validation ensures the structural and logical correctness of requests and is performed before business rules are evaluated.

**API validates:**
- Data type and format
- Time and logical consistency

> Requests that fail validation are rejected with a descriptive error response.

# Request fields and validation

> All request payloads must be valid JSON and use UTF-8 encoding.

| **Field**    | **Required / Optional** | **Data format** | **Example**             | **Validation rules**                                |
| ------------ | ----------------------- | --------------- | ----------------------- | --------------------------------------------------- |
| roomId       | Required                | string          | room-101                | Must reference an existing room ID                  |
| startTime    | Required                | ISO 8601 UTC    | 2026-01-15T10:00:00Z    | Must be earlier than endTime; cannot be in the past |
| endTime      | Required                | ISO 8601 UTC    | 2026-01-15T11:00:00Z    | Must be later than startTime; cannot be in the past |
| title        | Optional                | string          | Team Meeting            | Max length 255 characters                           |
| description  | Optional                | string          | Discuss project roadmap | Max length 1024 characters                          |
| participants | Optional                | integer         | 5                       | Must not exceed room capacity                       |

# Time validation rules

Time-based fields are validated to ensure logical consistency:

- The start time must be earlier than the end time  
- Bookings cannot be created in the past  
- Time values must be provided in UTC  

# Validation errors

Validation failures are mapped to standard HTTP status codes.

| Status code | Error code       | Description                          |
| ----------- | ---------------- | ------------------------------------ |
| 400         | INVALID_REQUEST  | Missing or invalid required field    |
| 400         | VALIDATION_ERROR | Field-level validation failure       |

> Full list of error codes and messages is in the [Error page](errors.md)

## Error response examples

**Validation error:**
```json
{
	"error": {
	    "code": "VALIDATION_ERROR",
	    "message": "Maximum length is 255 characters"
    }
}
```

**Invalid request:**
```json
{
  "error": "INVALID_REQUEST",
  "message": "Malformed JSON payload or missing required fields: roomId, startTime, endTime"
}
```

→ Read next [Business rules](business-rules.md)
