
| **Version** | **Date**   | **Changes**     |
| ----------- | ---------- | --------------- |
| 1.0.0       | 2025-10-01 | Initial release |

# Versioning policy

- The Smart Meeting Room Booking API uses **semantic versioning**: MAJOR.MINOR.PATCH.
    - **MAJOR** — incompatible changes
    - **MINOR** — new features, backward-compatible
    - **PATCH** — bug fixes, backward-compatible
- The current version is: **v1.0.0**
- Versioning is included in the **URL:** `https://api.example.com/v1/rooms`

# Backward compatibility notes

If a client uses an older API version, existing endpoints and request formats continue to work as defined in that version.

New features, fields, or behaviour changes are not available, and deprecated functionality may return warnings or be removed according to the deprecation policy.

# Update Recommendations

- Always check the version in the URL to ensure the client is using the intended API version
- Subscribe to API release notes to track new features or breaking changes
- Avoid relying on deprecated endpoints