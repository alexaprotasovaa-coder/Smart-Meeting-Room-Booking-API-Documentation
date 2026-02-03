**On this page:**
API features, use cases, authentication, and authorization summary.

**Not covered:**
Full authentication flow, detailed business rules, validation logic, advanced error handling, and edge cases.

# Introduction

The **Smart Meeting Room Booking API** is a RESTful API for managing meeting room reservations. It allows to confirm bookings or get a list of available rooms.

## Potential clients

Frontend apps, calendar integrations, and automated scheduling systems.

## What this API does

- Authorization and access
- Searching for available rooms 
- Validating requests
- Resolving conflicts using validation rules
- Creating a booking with automatic room selection
- Cancelling a booking

## What this API does not do

- Manage user identities
- Issue access tokens
- Handle UI-related logic
- Send calendar invitations
- Notify users of booking changes
- Support repetitive bookings
- Handle user management

# Use cases

## Actors

**Client**: 
submits a booking request

**API**: 
validates the request, selects a room (only in the "Create a booking" use case), checks conflicts, applies business rules, and returns a response

## 1. Create a booking

**Goal:**
Create a booking

**Flow:**
```
Client
   |
   |   POST /bookings
   ▼
  API
   |	 Validate token
   |    Validate request data: start time, end time, room id (optional)
   |    Select room
   |        - If the room id is specified, use it
   |        - Otherwise, automatically select a suitable room
   |     Check conflicts
   |     Apply business rules
   ▼
Response
```

**Result:** 
Booking is either **confirmed** or **rejected**

## 2. Search available rooms

**Goal:**
Find all available rooms

**Flow:**
```
Client
   |
   |  GET /rooms
   ▼
  API
   |	Validate token
   |   Validate request data: start time, end time
   |   Check conflicts
   |   Apply business rules 
   ▼
Response
```

**Result:**
API returns a list of available rooms (may be empty)

# Authentication and authorization summary

The **Smart Meeting Room Booking API** uses an **external IdP** for authentication. It does not handle credentials or issue tokens. The API accepts **Bearer tokens** via OAuth 2.0 and enforces **scope-based authorization** for client requests.

Authentication is required.
See [Authentication](authentication.md) for details.


→ Read next [Getting started](getting-started.md)
