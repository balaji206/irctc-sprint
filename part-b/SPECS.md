# Feature Specifications — IRCTC Sprint Part B

---

# Feature Spec 1: Tatkal Virtual Queue System

### Problem Statement
Millions of users try booking Tatkal tickets simultaneously causing server overload and crashes during the 10:00 AM booking window.

### Current State
Users experience freezes, session timeouts, CAPTCHA resets, payment failures, and unexpected logouts during Tatkal booking.

### Proposed Solution
Introduce a virtual waiting room with live queue positions, estimated wait time, and controlled entry into booking pages.

### Proposed User Flow
1. User opens Tatkal booking page.
2. System assigns a queue number automatically.
3. User sees estimated waiting time and live progress.
4. Queue updates in real-time.
5. User receives booking access when their turn arrives.
6. User completes payment safely within allocated time.

### Technical Implementation Plan

**System components affected:**
- Booking backend
- Authentication service
- Queue management service
- Frontend booking UI

**New data requirements:**
- Queue ID
- Queue token
- Queue expiry timestamp
- Booking session status

**API changes:**
- POST /queue/join
- GET /queue/status
- POST /queue/exit

**Frontend changes:**
- Queue waiting screen
- Live countdown timer
- Queue progress bar
- Session expiry alerts

**Third-party services:**
- Redis for real-time queue storage
- WebSockets for live updates

### Success Metrics
- Reduce Tatkal crash rate
- Increase booking completion rate
- Reduce repeated page refreshes
- Reduce payment failure complaints

### Edge Cases and Constraints
- Queue token expiration
- Sudden traffic spikes
- Payment gateway timeout
- Railway backend delays

---

# Feature Spec 2: Persistent Smart Search Filters

### Problem Statement
Search filters reset frequently and show incorrect train results, making train discovery slow and frustrating.

### Current State
Users apply filters such as class and availability, but results remain inconsistent or filters reset after page refresh.

### Proposed Solution
Create persistent smart filters that remain active across navigation and refreshes while synchronizing with live availability data.

### Proposed User Flow
1. User searches for trains.
2. User applies filters.
3. System stores filter preferences.
4. Results update dynamically.
5. User navigates between train details pages.
6. Filters remain preserved automatically.

### Technical Implementation Plan

**System components affected:**
- Search service
- Availability API
- Frontend search page

**New data requirements:**
- Saved filter preferences
- Cached search session

**API changes:**
- GET /search/results
- POST /filters/save

**Frontend changes:**
- Persistent filter chips
- Dynamic loading state
- Cached filter memory

**Third-party services:**
- Browser local storage

### Success Metrics
- Reduce search time
- Increase successful train discovery
- Reduce repeated filter usage

### Edge Cases and Constraints
- Cached stale data
- Network interruptions
- Live availability mismatches

---

# Feature Spec 3: Seat Lock and Confirmation System

### Problem Statement
Seat selections reset randomly during booking causing users to lose preferred berths.

### Current State
Users select seats but the booking form resets them to auto-assignment or assigns different seats.

### Proposed Solution
Introduce temporary seat locking with confirmation before proceeding to payment.

### Proposed User Flow
1. User selects seat preference.
2. System locks seat temporarily.
3. User sees confirmation popup.
4. User proceeds to passenger details.
5. Locked seat remains reserved.
6. User completes payment.

### Technical Implementation Plan

**System components affected:**
- Seat allocation service
- Booking flow backend
- Passenger details UI

**New data requirements:**
- Seat lock status
- Seat lock expiry timer
- Passenger-seat mapping

**API changes:**
- POST /seat/lock
- POST /seat/release
- GET /seat/status

**Frontend changes:**
- Locked seat indicator
- Countdown timer
- Seat confirmation popup

**Third-party services:**
- Redis cache for temporary locks

### Success Metrics
- Reduce seat reset complaints
- Increase successful seat preference retention
- Reduce back navigation during booking

### Edge Cases and Constraints
- Seat lock expiration
- Multiple users selecting same seat
- Payment failure after lock

---

# Feature Spec 4: Real-Time Payment Tracking System

### Problem Statement
Users do not receive proper payment status updates during UPI and card transactions.

### Current State
The payment page freezes with no clear status, causing panic, duplicate payments, and refund confusion.

### Proposed Solution
Provide real-time payment tracking with transaction status updates and automatic retry handling.

### Proposed User Flow
1. User selects payment method.
2. Payment process starts.
3. User sees live payment progress.
4. System checks payment gateway response.
5. Payment success or failure is displayed instantly.
6. Retry option appears if transaction fails.

### Technical Implementation Plan

**System components affected:**
- Payment gateway integration
- Booking backend
- Refund management system

**New data requirements:**
- Transaction status
- Retry count
- Refund tracking status

**API changes:**
- POST /payment/initiate
- GET /payment/status
- POST /payment/retry

**Frontend changes:**
- Payment progress tracker
- Retry button
- Transaction status popup

**Third-party services:**
- Razorpay / Paytm / UPI gateway APIs

### Success Metrics
- Reduce duplicate payments
- Reduce payment support complaints
- Improve successful transaction completion

### Edge Cases and Constraints
- Gateway downtime
- Slow bank response
- Duplicate transaction requests

---

# Feature Spec 5: Mobile Responsive Redesign

### Problem Statement
The IRCTC mobile website breaks on smaller screens causing layout overlap and difficult navigation.

### Current State
Users struggle with overlapping buttons, hidden text, and scrolling issues on mobile browsers.

### Proposed Solution
Redesign the mobile interface using responsive layouts optimized for low-end smartphones and slow networks.

### Proposed User Flow
1. User opens IRCTC on mobile.
2. Responsive layout loads automatically.
3. Navigation adapts to screen size.
4. Booking forms become touch-friendly.
5. Buttons remain visible during scrolling.
6. User completes booking smoothly.

### Technical Implementation Plan

**System components affected:**
- Frontend responsive UI
- Navigation system
- Mobile browser rendering

**New data requirements:**
- Device type detection
- Screen resolution preferences

**API changes:**
- No major API changes

**Frontend changes:**
- Responsive grid layout
- Mobile navigation drawer
- Sticky booking buttons
- Optimized typography

**Third-party services:**
- Tailwind CSS / Bootstrap responsive framework

### Success Metrics
- Reduce mobile bounce rate
- Improve mobile booking completion
- Increase accessibility scores

### Edge Cases and Constraints
- Very old Android devices
- Low-speed internet connections
- Browser compatibility issues

---

# Feature Spec 6: Refund and Cancellation Dashboard

### Problem Statement
Refund timelines and cancellation rules are difficult to understand causing confusion and support requests.

### Current State
Users cannot clearly track refund stages or understand deduction amounts after cancellation.

### Proposed Solution
Create a refund dashboard with live refund tracking and simplified cancellation explanations.

### Proposed User Flow
1. User opens booked ticket history.
2. User clicks cancelled ticket.
3. Dashboard shows refund timeline.
4. System displays deduction breakdown.
5. User sees expected refund date.
6. Notifications update refund progress.

### Technical Implementation Plan

**System components affected:**
- Refund management backend
- Ticket history page
- Notification service

**New data requirements:**
- Refund stage
- Refund ETA
- Cancellation deduction breakdown

**API changes:**
- GET /refund/status
- GET /refund/timeline

**Frontend changes:**
- Refund progress tracker
- Timeline visualization
- FAQ explanation section

**Third-party services:**
- SMS/email notification APIs

### Success Metrics
- Reduce refund-related support tickets
- Improve refund transparency
- Increase user trust in cancellations

### Edge Cases and Constraints
- Bank processing delays
- Failed refund transfers
- Partial refunds