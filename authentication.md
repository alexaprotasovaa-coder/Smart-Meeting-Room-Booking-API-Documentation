**On this page:**
Instruction of how to authenticate and authorise requests to the **Smart Meeting Room Booking API** using an **external identity provider (IdP)** via **OAuth 2.0**.  

# External identity provider (IdP)

The API relies on an **external identity provider (IdP)** to handle authentication and token issuance. The API does not handle token issuance.
For more information, see [IDP documentation](https://auth0.com/docs/secure/tokens/access-tokens/identity-provider-access-tokens).

Currently, the API integrates with **Auth0**, which:  
- Validates client credentials  
- Issues with **OAuth 2.0 access tokens** with scopes  
- Enforces secure authentication without storing credentials in the API

# OAuth 2.0 flow

| **Step** | **Action**            | **Notes**                                   | **Example**       |
| -------- | --------------------- | ------------------------------------------- | ----------------- |
| 1        | Authenticate with IdP | Token issued with scopes                    | POST /oauth/token |
| 2        | Store token securely  | HTTPS only, do not log                      | —                 |
| 3        | Make API request      | Include ```Authorization: Bearer <token>``` | GET /rooms        |

**Request example:**
```
curl -X GET https://api.smart-meeting-room-booking.com/rooms \
  -H "Authorization: Bearer <access_token>"
```

# Access tokens

The API requires a valid **Bearer token** issued by the external IdP. For more information on how tokens are managed and configured, see the [Auth0 documentation](https://auth0.com/docs).

> API usage limits depend on the client’s access level

### Token lifetime

> Access tokens are valid for **60 minutes** from the time of issuance

- The API does not automatically refresh expired tokens. Clients must manually request a new token
- Requests made with an expired access token are rejected
- Clients must request a new token from the IdP before making further API calls

### Authorization header

Request to the API should always include `Authorization: Bearer <token>` header.

# Authorization scopes

**Scopes:**
- Are attached to **Access tokens**
- Are validated on every request (API returns an error if the required scope is missing)
- Control **access to API operations**

**List of scopes:**

| **Scope**      | **Description**              |
| -------------- | ---------------------------- |
| rooms:read     | View available meeting rooms |
| bookings:read  | View existing bookings       |
| bookings:write | Create or cancel bookings    |

# Authentication and Authorization Errors

The API uses standard HTTP status codes to indicate authentication and authorization errors.

| **Status Code** | Error code         | **Description**                      |
| --------------- | ------------------ | ------------------------------------ |
| 401             | UNAUTHORIZED       | Missing or invalid access token      |
| 403             | INSUFFICIENT_SCOPE | Missing required authorization scope |

## Error response examples

**Missing token:**
```json
 {
	"error": {
		"code": "UNAUTHORIZED",
	    "message": "Access token is missing or invalid."
	}
}
```
> A valid token does not guarantee access; the required scope must be present.

**Expired token:**
```HTTP
HTTP/1.1 401 Unauthorized
Content-Type: application/json
```
```json
{
	"error": {
	    "code": "TOKEN_EXPIRED",
	    "message": "The access token has expired. Please obtain a new token and retry the request."
	}
}
```

**Insufficient scope:**
```HTTP
HTTP/1.1 403 Forbidden
Content-Type: application/json
```
```json
{
	"error": {
		"code": "INSUFFICIENT_SCOPE",
		"message": "The access token does not include the required scope: bookings:write."
	}
}
```

**Invalid request structure:** 
Request
```
curl -X POST https://api.smart-meeting-room-booking.com/bookings \
  -H "Authorization: Bearer <access_token>" \
  -H "Content-Type: application/json" \
  -d '{"roomId": "", "startTime": "", "endTime": ""}'
```
Response
```json
{
  "error": {
    "code": "INVALID_REQUEST",
    "message": "Required fields are missing or invalid: roomId, startTime, endTime."
  }
}
```

