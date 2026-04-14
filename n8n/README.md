# AKAR CRM Dealership Operating System (DOS) - n8n Package

This folder contains 8 modular workflows for AKAR CRM using one Google Sheets database (`AKAR CRM MASTER`).

## Included Workflows
- `WF1_Lead_Capture.json`
- `WF2_FollowUp_Engine.json`
- `WF3_Quotation_Engine.json`
- `WF4_Payment_Control.json`
- `WF5_Inventory_VIN.json`
- `WF6_Delivery_Control.json`
- `WF7_Notifications.json`
- `WF8_Dashboard_Integrity.json`

## Google Sheets Tabs Required
Create these tabs with exact headers:

1. Leads: `leadId, name, phone, model, variant, assignedTo, teamLeader, manager, stage, followUpCount, nextFollowUpDate`
2. FollowUp_Log: `leadId, date, remarks, nextFollowUp, doneBy`
3. Quotations: `quotationId, leadId, model, variant, exShowroom, discount, rto, insurance, accessories, finalPrice`
4. Bookings: `bookingId, leadId, model, variant, vin, amount, totalPaid, pending, status, createdAt`
5. Payments: `paymentId, bookingId, amount, mode, date, receivedBy`
6. Inventory: `vin, model, variant, color, fuel, status, age, location`
7. Waiting_List: `leadId, model, variant, priority, date`
8. Delivery_Control: `bookingId, financeStatus, accessoriesStatus, pdiStatus, paymentStatus, nocStatus`
9. Users_Master: `name, role, teamLeader, manager, telegramChatId`
10. Dashboard_Data: `timestamp, leadToBookingPct, bookingToDeliveryPct, stockAgingHigh, pendingPayments`

## Key Guardrails Implemented
- No booking payment accepted without valid `bookingId` (WF4).
- No delivery without payment cleared + finance + accessories + PDI (WF6).
- VIN allocation only from AVAILABLE stock, cancellation releases VIN (WF5).
- Quotation uses strict formula and discount cap enforcement, no manual final price override (WF3).
- Follow-up escalation after 3 attempts and scheduler reminders every 2 hours (WF2).
- Integrity monitoring to detect VIN anomalies and hidden bookings with GM alerts (WF8).

## Credentials / Environment
- Replace `AKAR_CRM_MASTER_SPREADSHEET_ID` in each workflow.
- Set Telegram bot token in n8n environment as `TELEGRAM_BOT_TOKEN`.
- Configure Google Sheets OAuth credentials for all Google Sheets nodes.

## Import Order
Import WF7 first (shared notification dependency), then WF1-WF6, then WF8.
