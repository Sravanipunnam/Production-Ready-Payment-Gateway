# Webhook Testing Guide

## What You're Seeing

The webhook logs showing "failed" status with 0 attempts is expected behavior. Here's why:

### Why Webhooks Show as "Failed" with 0 Attempts

When a payment is processed, the system tries to deliver a webhook. However, if the merchant doesn't have a `webhook_url` configured, the system:
1. Creates a webhook log entry
2. Sets status to "failed" (because there's no URL to deliver to)
3. Sets attempts to 0 (because no delivery attempt was made)
4. Skips the actual webhook delivery

This is by design - the system logs that a webhook event occurred, but since there's no URL configured, it can't deliver it.

## Testing Webhooks Properly

To test webhooks with actual delivery, you have two options:

### Option 1: Use a Webhook Testing Service

1. Go to https://webhook.site (or similar service)
2. Copy your unique webhook URL (e.g., `https://webhook.site/unique-id-here`)
3. In the dashboard, paste this URL into the "Webhook URL" field
4. Click "Save Configuration" (note: this feature needs backend API implementation)
5. Create a new payment - the webhook should be delivered to your webhook.site URL

### Option 2: Test Locally with a Webhook Receiver

1. Set up a local webhook receiver
2. Use `http://host.docker.internal:PORT/webhook` as the webhook URL
3. Configure it in the dashboard
4. Test webhook delivery

## Current Status

✅ **Frontend is working correctly:**
- Webhook configuration page displays
- Webhook logs table shows data
- Retry buttons are visible
- All UI elements are functional

⚠️ **Backend Configuration Needed:**
- The "Save Configuration" button needs backend API endpoint to update merchant's webhook_url
- Currently, webhooks fail because no webhook_url is set for the test merchant

## Understanding the Webhook Logs

Your logs show:
- **payment.success** (2 entries) - Payments that succeeded
- **payment.failed** (1 entry) - A payment that failed
- **Status: failed** - Because no webhook_url is configured
- **Attempts: 0** - No delivery attempts made (skipped due to missing URL)
- **Retry button** - Available but will fail again until webhook_url is configured

## Next Steps to Test Full Webhook Flow

1. **Configure Webhook URL** (requires backend API implementation for saving)
2. **Create a new payment** - This will trigger a new webhook
3. **Check webhook.site** - You should see the webhook payload
4. **Verify signature** - Check the X-Webhook-Signature header matches

## Testing the Retry Button

You can test the retry button:
1. Click "Retry" on any webhook log entry
2. Check browser console (F12) for API calls
3. Check worker logs: `docker compose logs worker`
4. The retry will still fail until webhook_url is configured, but you can verify the API call works
