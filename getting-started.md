**On this page:**
Ability to authenticate, send a basic request, and receive a valid response from the API.

**Not covered:**
Full authentication flow, detailed business rules, validation logic, advanced error handling, and edge cases.

# Base URL and API version

Base URL for all API requests: `https://api.smart-meeting-room-booking.com/v1`

# Authentication requirements

- All requests require a valid OAuth 2.0 Bearer access token issued by an external identity provider
- The API does not manage credentials or issue tokens
- Access is controlled using scope-based authorization

See [Authentication](authentication.md) for details.

# Minimal request flow

1. Obtain an access Auth0 token from the external identity provider
2. Send a request to the API with the `Authorization: Bearer <token>` header
3. The API validates the request and returns a response

# Minimal example

This example shows how to create a booking with automatic room selection.
If the request cannot be fulfilled, the API returns a clear error response.

Request
```http
POST /api/bookings HTTP/1.1
Host: api.smart-meeting-room-booking.com
Content-Type: application/json
Authorization: Bearer <access_token>
```
```json
{
  "room_id": "101",
  "user_id": "u123",
  "start_time": "2026-01-10T10:00:00Z",
  "end_time": "2026-01-10T12:00:00Z"
}
```

Response (Success, 201 Created)
```json
{
  "booking_id": "b456",
  "room_id": "101",
  "user_id": "u123",
  "start_time": "2026-01-10T10:00:00Z",
  "end_time": "2026-01-10T12:00:00Z",
  "status": "confirmed"
}
```

Response (Error, 409 Conflict)
```json
{
  "error": "TIME_CONFLICT",
  "message": "The requested time slot overlaps with an existing booking."
}
```
