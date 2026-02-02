**On this page:**
How the API reports errors and how clients should interpret them.

> Error handling is consistent across all endpoints and aligned with the authentication, validation, and business rules described in previous sections

## Error response format

All errors are returned as a JSON object with a predictable structure.

```json
{
	"error": {
	    "code": "ERROR_CODE",
	    "message": "Human-readable description of the error",
	    "details": []
	}
}
```

**Fields:**
- `code` — machine-readable error identifier
- `message` — concise description suitable for logs and UI messages
- `details` — optional array with field-level or contextual information. The details field is included only when additional context is required

## Error lifecycle

Errors are evaluated and returned according to the request processing lifecycle:

1. **Request validation** - Structural and data-level checks
2. **Authorization** - Token validity and required scopes
3. **Business rules** - Domain constraints and policies
4. **Conflict resolution** - Availability and priority-based decisions

Once an error occurs, request processing stops and the error response is returned immediately.

## Common errors

Common errors apply across all API operations.

|**Status code**|**Error code**|**Description**|
|---|---|---|
|400|INVALID_REQUEST|Malformed or invalid request payload|
|400|MISSING_REQUIRED_DATA|Required fields are missing|
|401|UNAUTHORIZED|Missing or invalid access token|
|403|FORBIDDEN|Access is not permitted|
|404|RESOURCE_NOT_FOUND|Requested resource does not exist|
|429|RATE_LIMIT_EXCEEDED|Request rate limit exceeded|
|500|INTERNAL_SERVER_ERROR|Unexpected server-side error|

## Domain-specific errors
 
Domain-specific errors reflect booking logic, policies, and time constraints.

|**Status code**|**Error code**|**Description**|
|---|---|---|
|404|ROOM_NOT_FOUND|The specified room does not exist|
|403|POLICY_VIOLATION|Request violates system policy rules|
|409|TIME_CONFLICT|Booking conflicts with an existing booking|
|409|TOO_LATE_TO_CANCEL|Cancellation attempted too close to start time|
|422|INVALID_TIME_RANGE|Time range is logically invalid|

> Domain-specific errors are returned only after successful validation and authorization

## Error examples

**Validation error**
```json
{
	"error": {
	    "code": "VALIDATION_ERROR",
	    "message": "One or more fields failed validation",
	    "details": [
		    {
	        "field": "startTime",
	        "issue": "startTime must be earlier than endTime"
		    }
	    ]
	}
}
```

**Authorization error**
```json
{
	"error": {
	    "code": "UNAUTHORIZED",
	    "message": "Access token is missing or invalid"
	}
}
```

**Time conflict error**
```json
{
	"error": {
	    "code": "TIME_CONFLICT",
	    "message": "The requested time range overlaps with an existing booking"
	}
}
```

**Policy violation error**
```json
{
	"error": {
	    "code": "POLICY_VIOLATION",
	    "message": "The request violates booking policy rules"
	}
}
```
