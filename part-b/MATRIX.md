# Impact vs Effort Matrix

|                   | Low Effort | High Effort |
|-------------------|-------------|-------------|
| High Impact | Search Filter Fix | Tatkal Queue System |
| Low Impact | Refund Dashboard | AI Prediction System |

---

# Placement Justifications

## Tatkal Queue System — High Impact / High Effort
Affects millions of users daily during Tatkal booking.
Requires backend queue infrastructure and real-time updates.
Should be implemented as a long-term major project.

## Search Filter Persistence — High Impact / Low Effort
Affects almost every train search user.
Only frontend state management and API caching changes are required.
Can be implemented quickly with large UX improvement.

## Seat Lock System — High Impact / Medium Effort
Users lose selected seats during booking.
Requires frontend and backend synchronization.
Improves trust during booking flow.

## Payment Tracker — High Impact / Medium Effort
Users panic when payments freeze.
Needs payment status APIs and retry handling.
Reduces duplicate payments and refund requests.

## Mobile Responsive Redesign — Medium Impact / Medium Effort
Mobile users face layout issues on smaller screens.
Requires responsive UI redesign.
Improves accessibility for mobile-first users.

## Refund Dashboard — Low Impact / Low Effort
Users struggle understanding refund timelines.
Mostly UI and information architecture improvements.
Easy to implement with moderate user benefit.