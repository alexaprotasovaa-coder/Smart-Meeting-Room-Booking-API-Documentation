**On this page:**
Сore domain concepts used throughout the **Smart Meeting Room Booking API**.

## **Booking**

A **booking** represents a reservation of a meeting room for a specific time range.

**Features:**
- Is associated with exactly one room
- Has a defined start and end time
- Represents exclusive use of the room during that time range
- May include additional metadata such as title, description, or participants

A booking exists only within its defined time range and does not extend beyond it.

## **Room**

A **room** is a physical or virtual meeting space that can be reserved.

**Is defined by:**
- A unique identifier (id)
- A maximum capacity indicating how many attendees it can accommodate
- A set of attributes describing the room

Room properties are used to determine whether a booking can be created or accepted.

## **Time range**

A **time range** defines the period during which a booking is active.

**Features:**
- Consists of a startTime and an endTime
- Is interpreted as a continuous, non-inclusive interval
- Must represent a valid chronological order 

All time ranges are evaluated consistently to determine availability and conflicts.

## **Availability**

**Availability** describes whether a room can be booked for a given time range.

**A room is considered available if:**
- No existing booking occupies the same time range
- The requested time range satisfies system constraints

Availability is evaluated dynamically based on existing bookings and time rules.

## **Conflict**

A **conflict** occurs when two or more bookings attempt to reserve the same room for overlapping time ranges.

**Conflicts:**
- Are evaluated per room
- Are determined solely by time range overlap
- Prevent simultaneous use of the same room

Conflict detection ensures exclusive access to rooms.

## **Priority**

**Priority** represents the relative importance of a booking request.

**Features:**
- Is expressed as a numeric value within a defined range
- Is used to resolve competing booking requests
- Influences booking decisions when conflicts occur 

## **Client**


A **Client** is an application or system that interacts with the API on behalf of a user or organisation.

- Authenticates via an external identity provider
- Obtains access tokens with assigned scopes
- Initiates API operations such as room search and booking creation
- Is subject to authorization rules and system policies

**A client may represent:**
- A web application
- A mobile application
- An internal service or integration

All API interactions are performed by clients, not end users directly.