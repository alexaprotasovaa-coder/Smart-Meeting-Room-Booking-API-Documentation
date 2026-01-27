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

| **Field**    | **Required / Optional** | **Data format** | **Example**             | **Validation rules**                                | **Business rules impact**                    |
| ------------ | ----------------------- | --------------- | ----------------------- | --------------------------------------------------- | -------------------------------------------- |
| roomId       | Required                | string          | room-101                | Must reference an existing room ID                  | Used for room selection, availability checks |
| startTime    | Required                | ISO 8601 UTC    | 2026-01-15T10:00:00Z    | Must be earlier than endTime; cannot be in the past | Conflict detection, time constraints         |
| endTime      | Required                | ISO 8601 UTC    | 2026-01-15T11:00:00Z    | Must be later than startTime; cannot be in the past | Conflict detection, time constraints         |
| title        | Optional                | string          | Team Meeting            | Max length 255 characters                           | Display only; no effect on business rules    |
| description  | Optional                | string          | Discuss project roadmap | Max length 1024 characters                          | Display only; no effect on business rules    |
| participants | Optional                | integer         | 5                       | Must not exceed room capacity                       | Capacity check for room selection            |

# Supported data formats

> Data format includes both the data type and, where applicable, a specific format (e.g., ISO 8601 for dates)

- `string`
- `integer`
- `boolean`
- `array`
- `object`
- `ISO 8601` date-time strings

**Examples:**
- `roomId: string`  
- `participants: integer`  
- `startTime: ISO 8601` (e.g. `2026-01-15T10:00:00Z`)  

# Time validation rules

Time-based fields are validated to ensure logical consistency:

- The start time must be earlier than the end time  
- Bookings cannot be created in the past  
- Time values must be provided in UTC  

# Validation errors

Validation failures are mapped to standard HTTP status codes.

| Status code | Error code       | Description                          |
| ----------- | ---------------- | ------------------------------------ |
| 400         | INVALID_REQUEST  | Malformed or invalid request payload |
| 400         | VALIDATION_ERROR | Field-level validation failure       |

## Error response examples

**Validation error:**
```json
{
	"error": {
	    "code": "VALIDATION_ERROR",
	    "message": "One or more fields failed validation.",
	    "details": [
		    {
		        "field": "startTime",
		        "issue": "startTime must be earlier than endTime"
			}
	    ]
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