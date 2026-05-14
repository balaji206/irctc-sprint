# IRCTC Problem Discovery — Part A

## Summary

- Total problems documented: 6 (3 given + 3 self-discovered)
- Platform explored: https://www.irctc.co.in
- Devices used:
  - Desktop Chrome
  - Mobile Chrome

---

# Problem 1: Tatkal Booking Crashes at 10:00 AM [Given]

**Category:** Performance / Infrastructure / UX

## What is broken

The IRCTC Tatkal booking system becomes unresponsive exactly when Tatkal booking opens at 10:00 AM. Users face freezes, HTTP 502 errors, delayed OTP delivery, session logout, and payment uncertainty during the highest-demand booking window.

The system provides no queue visibility or progress feedback, causing repeated clicks that worsen server overload.

## Affected users

- Tatkal users across India
- Estimated 20–40 lakh concurrent users during 9:58–10:05 AM
- Strongly affects students, migrant workers, emergency travelers, and Tier 2/Tier 3 users dependent on train travel

## Frequency

- Daily
- Happens every morning during Tatkal opening
- Most severe between 10:00–10:05 AM

## Current flow — step by step

1. User opens IRCTC at 9:50 AM
2. User logs in and searches trains
3. User selects Tatkal quota
4. Availability shows “Available 12”
5. User fills passenger details before 10 AM
6. User clicks “Book Now” at 10:00 AM
7. Loading spinner appears with no progress feedback
8. User waits 15–45 seconds
9. Session crashes / CAPTCHA resets / HTTP 502 occurs
10. User refreshes and finds Tatkal quota exhausted

## Where exactly it breaks

**Step 7–9**

The booking request hits overloaded backend servers simultaneously with millions of requests. The frontend provides no queue state or retry handling, causing uncertainty and repeated submissions.

## Impact

- Ticket loss despite reaching payment stage
- Payment confusion and refund anxiety
- Users repeatedly log in and restart the process
- High emotional frustration during urgent travel needs

---

# Problem 2: Search Filters Do Not Work Reliably [Given]

**Category:** UX / Information Architecture

## What is broken

Search filters for class, quota, availability, and departure time behave inconsistently. Results frequently ignore filters or reset after page reloads.

Users cannot trust the filtered train list.

## Affected users

- All train-search users
- Senior citizens and first-time users rely heavily on filters
- Users booking during peak periods experience the issue more often

## Frequency

- Happens intermittently
- Estimated 30–40% failure rate during heavy traffic

## Current flow — step by step

1. User searches trains between two cities
2. Results display 20–40 trains
3. User selects “Sleeper” and “Available”
4. Page reloads
5. Waitlisted trains still appear
6. User opens a train detail page
7. Train shows “WL 34” despite “Available” filter
8. User clicks Back
9. Filters reset to default

## Where exactly it breaks

**Step 4–9**

Filters appear to work client-side on stale cached data. When live data refreshes, the filter state is lost and incorrect train results are shown.

## Impact

- Users waste 8–15 minutes manually scanning trains
- Reduced trust in IRCTC search reliability
- Higher booking abandonment

---

# Problem 3: Seat Selection Resets Randomly [Given]

**Category:** UX / Mobile / State Management

## What is broken

Seat preferences selected on the seat map are randomly reset before booking confirmation. Users selecting lower berths often receive different seats or automatic allocation.

## Affected users

- Elderly passengers
- Families with children
- Divyang users
- Mobile users especially affected

## Frequency

- Desktop: ~12% sessions
- Mobile: ~35% sessions

## Current flow — step by step

1. User selects train and class
2. Seat map loads
3. User selects lower berth
4. Selected berth turns blue
5. User clicks “Proceed”
6. Passenger details page loads
7. Seat preference changes to “Auto”
8. User goes back
9. Previously selected seat becomes unavailable

## Where exactly it breaks

**Step 5–7**

Seat selection state is not consistently preserved between seat map and passenger form components. Mobile re-rendering clears local selection state.

## Impact

- Elderly passengers receive upper berths
- Users lose trust in seat selection
- Multiple retries increase booking time

---

# Problem 4: Payment Page Freezes During UPI Transactions [Self-Discovered]

**Category:** Payment / Mobile UX / Performance

## What is broken

During UPI payments, the payment gateway freezes after clicking “Pay”. Users receive no live payment confirmation and are unsure whether payment succeeded or failed.

## Affected users

- Mobile users using UPI
- Students and younger travelers using Google Pay, PhonePe, and Paytm
- High impact during peak booking hours

## Frequency

- Observed 2 out of 5 attempts
- More common during heavy traffic

## How I found it

I proceeded to the payment page after selecting a train and tested the UPI payment option on mobile Chrome.

## Screenshot or description

The payment screen showed a loading spinner for over 25 seconds after clicking “Pay via UPI,” with no success or failure message displayed.

## Current flow — step by step

1. User completes passenger details
2. User reaches payment page
3. User selects UPI payment
4. User enters UPI ID
5. User clicks “Pay”
6. Spinner appears
7. No payment status shown
8. User waits without feedback
9. User refreshes page
10. Booking session expires or payment status becomes unclear

## Where exactly it breaks

**Step 6–8**

The frontend lacks real-time transaction status updates from the payment gateway. Session timeout and delayed callback handling worsen uncertainty.

## Impact

- Duplicate payment attempts
- Refund cycles lasting several days
- Users abandon bookings
- Increased support complaints

---

# Problem 5: Mobile Website Navigation Breaks on Smaller Screens [Self-Discovered]

**Category:** Mobile / Accessibility / Responsive Design

## What is broken

The IRCTC mobile website becomes difficult to navigate on smaller screens. Buttons overlap, dropdowns extend beyond the viewport, and important booking actions are hidden below banners.

## Affected users

- Android users on budget smartphones
- Users with slow mobile internet
- Older users unfamiliar with zoom and horizontal scrolling

## Frequency

- Consistent on small-screen devices
- Worse in portrait mode

## How I found it

I opened the IRCTC website on mobile Chrome and attempted a train booking flow on a smaller screen device.

## Screenshot or description

The train filter section overlapped the search results area, and the “Continue Booking” button required horizontal scrolling to access fully.

## Current flow — step by step

1. User opens IRCTC mobile site
2. User searches trains
3. Search results load
4. User opens filters
5. Filter panel overlaps content
6. User scrolls horizontally accidentally
7. Important buttons become partially hidden
8. User zooms manually
9. Booking flow becomes difficult to continue

## Where exactly it breaks

**Step 4–7**

Responsive layout handling fails for smaller viewports, especially when dynamic banners or popup sections load.

## Impact

- Higher mobile abandonment rate
- Increased accidental taps
- Slower booking completion
- Poor accessibility for elderly users

---

# Problem 6: Refund and Cancellation Information Is Difficult to Understand [Self-Discovered]

**Category:** Information Architecture / UX

## What is broken

Cancellation charges, refund timelines, and TDR filing rules are spread across multiple pages with complex wording. Users struggle to understand how much refund they will actually receive.

## Affected users

- First-time travelers
- Senior citizens
- Users canceling emergency bookings
- Non-technical users unfamiliar with railway terminology

## Frequency

- Always present during cancellation flow

## How I found it

I explored the cancellation and refund section after reviewing booked ticket history and TDR information pages.

## Screenshot or description

Refund policies were displayed as long text-heavy paragraphs with multiple links and no refund calculator or simplified explanation.

## Current flow — step by step

1. User opens booked ticket history
2. User selects a booked ticket
3. User clicks “Cancel Ticket”
4. Cancellation rules page opens
5. User sees multiple refund conditions
6. User opens TDR policy link
7. Refund amount still unclear
8. User searches external websites for explanation
9. User returns to cancel booking

## Where exactly it breaks

**Step 4–7**

Information architecture is fragmented across multiple pages with legal-style wording and no clear refund breakdown or summary.

## Impact

- User confusion
- Cancellation hesitation
- Increased customer support dependency
- Users rely on third-party websites for clarification

---

# Final Notes

## Devices Tested

- Desktop Chrome
- Android Chrome mobile browser

## Exploration Method

- Live testing on IRCTC
- Multiple train searches
- Payment flow exploration
- Mobile responsiveness testing
- Cancellation and refund navigation testing