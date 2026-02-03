**On this page:**
Explanation of how the system applies business rules using the core domain concepts: **Booking**, **Room**, **Time range**, **Availability**, and **Conflict**.

**Not covered:**
- Request validation
- Technical error handling


> Notes
> 1. All business rules are enforced server-side
> 2. Client-side validation is optional and should be considered advisory only

# Room selection logic

When creating a booking, the system automatically selects a room.

A suitable room must:
- Meet the required capacity
- Be available for the requested time range
- Not conflict with existing bookings

Room selection is performed only after all validation rules have passed.

# Availability rules

Availability defines whether a room can be used for a given time range.

A room is considered **available** if:
- No existing booking occupies the same time range
- The requested time range satisfies all time constraints
- The room capacity meets the booking requirements

> Room capacity is defined on the [Concepts](concepts.md) page

Availability is evaluated before conflict resolution.

# Conflict detection

A **conflict** occurs when two bookings request the same room for overlapping time ranges. When a conflict is detected, the system immediately rejects the request.

# Automation & system behaviour

The system automatically:
- Selects a suitable room during booking creation
- Evaluates availability and detects conflicts

Depending on the operation:
- Room search returns a list of available rooms
- Booking creation results in a single room selection

# Policy violations

Requests that violate policy rules are rejected regardless of availability or room selection results.

# Decision table

| **Condition**                                                                   | **Action**     |
| ------------------------------------------------------------------------------- | -------------- |
| Time ranges do not overlap in any room                                          | Accept booking |
| Time ranges overlap in the same room                                            | Reject booking |
| Any booking request violates policy rules                                       | Reject booking |

# Time constraints

The system enforces the following time-related rules:

The system does not allow:
- Creating bookings in the past
- Cancelling bookings less than 10 minutes before the scheduled start time

The system requires:
- Bookings to last at least 15 minutes
- All time values to be provided in UTC

# Booking decision flow

```
New booking request
        |
        v
Evaluate time range
        |
        v
Check room availability
        |
        v
Is there an existing booking?
        |
   -------------------------
   |                       |
   No                     Yes
   |                       |
   v                       v
Accept booking      Detect time overlap
                           |
                ---------------------
                |                   |
            No overlap           Overlap
                |                   |
                v                   v
        Accept booking       Reject booking
```

→ Read next [Errors](errors.md)
