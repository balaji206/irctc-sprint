# AI Feature Specification: Waitlist Confirmation Predictor

## Problem It Solves
Users cannot predict whether waitlisted tickets will get confirmed.

## Proposed Feature — User Perspective
Users see a confirmation probability percentage before booking tickets.

## Model or API Choice
XGBoost machine learning model trained on historical booking data.

## Training or Input Data
- Historical waitlist data
- Cancellation trends
- Festival season demand
- Train occupancy rates

## How Output Is Shown to the User
Example:
WL 24 → 82% confirmation chance

## Confidence Threshold and Fallback
If confidence is below 60%, system shows:
"Prediction unavailable."

## Success Metrics
- Reduce user uncertainty
- Increase successful booking decisions
- Reduce cancellation confusion

## Limitations and Risks
Predictions may fail during holidays or unexpected train cancellations.