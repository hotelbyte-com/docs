---
name: General
about: For general questions
title: ''
labels: ''
assignees: Danceiny

---

# HotelByte Issue Submission Template
> ⚠️ Important: This template is for non-urgent issues only. P0 bugs (critical production issues with ≥$100 financial loss) must be reported via [email](mailto:urgent@hotelbyte.com) or instant messaging (e.g., WhatsApp) instead of submitting an issue.

## 1. Issue Type (Please select ONE, *required*)
- [ ] Bug (Technical defect, refer to severity definitions below)
- [ ] Task (Inquiry/consultation)
- [ ] Feature (New feature request)

## 2. Issue Details
### Basic Information
- **Title**: <span style="color:red">*[Format hint: Type + Severity (if Bug) + Brief Description]*</span>  
  Example:  
  - Bug: "P1: Credit card payment fails on booking confirmation page"  
  - Task: "Inquiry: How to integrate v3 API with existing booking system"  
  - Feature: "Feature: Add multi-language support for user profiles"
- **Related Module**: <span style="color:red">*[Select from dropdown or add custom: Search Function / Booking Flow / Payment System / User Profile / Notification Service / Reporting Tool / API Integration / Other]*</span>
- **Environment**: <span style="color:red">*[Auto-fill hint: Production/Staging/Test + Version (e.g., Production v2.3.1) + Region (if applicable)]*</span>

### Detailed Description (必填, *required*)
<span style="color:red">*Please provide structured information based on the issue type:*</span>
- For Bugs:
  1. Steps to reproduce: [e.g., 1. Go to booking page; 2. Select 2-night stay; 3. Enter credit card details; 4. Click "Confirm Payment"]
  2. Actual result: [e.g., Page shows "Server Error 500" and payment is deducted but booking not created]
  3. Expected result: [e.g., Booking confirmed, receipt sent, and no duplicate charges]
  4. Error logs/Screenshots: [Paste direct links or attach files (e.g., Sentry log URL, screen recording)]
  5. Affected scope: [e.g., All users paying with Visa; 120+ reported cases in EU region]
- For Tasks:
  1. Specific question: [e.g., What authentication method is required for the new inventory API?]
  2. Background: [e.g., Our team is building a third-party channel manager and needs to sync inventory]
  3. Required support: [e.g., API docs link, sample auth code, 30-min technical sync]
- For Features:
  1. Function description: [e.g., Add "book now pay later" option with 30-day installment]
  2. Use case: [e.g., Business travelers need flexible payment terms for corporate bookings]
  3. Expected value: [e.g., Reduce booking abandonment rate by 15%; attract 20k+ new corporate users]

### Additional Attachments (Optional)
[Add screenshots, network logs, system reports, or mockups here to speed up processing]

## 3. Response & Resolution Timelines
| Issue Type               | Response Time | Resolution/Plan Timeline          |
|--------------------------|---------------|-----------------------------------|
| Production P1 Bug        | Within 24hrs  | Fix & deploy within 24hrs         |
| Production P2 Bug        | Within 2 days | Fix & deploy within 7 days        |
| Other Production Bugs    | Within 7 days | Plan & update within 15 days      |
| Task (Inquiry)           | Within 3 days | Action plan timeline provided     |
| Feature Request          | Within 7 days | Feature plan finalized within 14 days |

## 4. Bug Severity Definitions (For Bug Type Only)
- P0: Involves financial losses exceeding USD 100 (report via [urgent email](mailto:urgent@hotelbyte.com) instead)
- P1: Involves financial losses; blocks core search/booking workflow; ≥1,000 error requests
- P2: Non-blocking issues outside core search/booking workflow; 100–999 error requests
- Other: Minor defects with no financial impact or limited user impact

---
