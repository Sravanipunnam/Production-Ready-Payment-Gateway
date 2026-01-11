# Frontend Testing Guide

## 🎯 Overview

You have two frontend services to test:
1. **Dashboard** (React) - http://localhost:3000
2. **Checkout Service** (Express) - http://localhost:3001

---

## 📊 Dashboard Testing (http://localhost:3000)

### 1. Access the Dashboard
- Open your browser and navigate to: **http://localhost:3000**
- You should see the navigation menu at the top

### 2. Webhook Configuration Page
- Click on **"Webhooks"** link in the navigation (or go to: http://localhost:3000/dashboard/webhooks)

#### What to Check:
✅ **Page loads without errors**
- No console errors in browser DevTools (F12)
- Page title shows "Webhook Configuration"

✅ **Webhook URL Input**
- You should see an input field for "Webhook URL"
- Field should be empty initially

✅ **Webhook Secret Display**
- Should show: `whsec_test_abc123`
- "Regenerate" button should be visible

✅ **Action Buttons**
- "Save Configuration" button (form submit)
- "Send Test Webhook" button

✅ **Webhook Logs Table**
- Table should display with columns:
  - Event
  - Status
  - Attempts
  - Last Attempt
  - Response Code
  - Actions
- You should see at least one log entry (from previous payment tests)
- If there are failed webhooks, "Retry" button should appear

#### Test Actions:
1. **View Logs**: Check if webhook logs from previous API tests are visible
2. **Try Regenerate**: Click "Regenerate" button - secret should change
3. **Enter Webhook URL**: Type a test URL (e.g., `https://webhook.site/your-unique-id`)
4. **Save Configuration**: Click "Save Configuration" (may show alert - implementation needed)
5. **Test Webhook**: Click "Send Test Webhook" (may show alert - implementation needed)
6. **Retry Failed Webhook**: If any logs show status "failed", click "Retry" button

### 3. API Documentation Page
- Click on **"API Documentation"** link in navigation (or go to: http://localhost:3000/dashboard/docs)

#### What to Check:
✅ **Page loads without errors**
- No console errors
- Three sections should be visible:
  1. Create Order
  2. SDK Integration
  3. Verify Webhook Signature

✅ **Code Snippets**
- Each section should have code examples in formatted blocks
- Code should be readable and properly formatted

✅ **Content Verification**
- Section 1: Shows curl command for creating orders
- Section 2: Shows SDK integration code with PaymentGateway class
- Section 3: Shows Node.js code for webhook signature verification

---

## 🛒 Checkout Service Testing (http://localhost:3001)

### 1. Access Checkout Page
- Open browser and go to: **http://localhost:3001/checkout?order_id=test123&key=key_test_abc123&embedded=false**

#### What to Check:
✅ **Page loads**
- Checkout form should appear
- Title: "Complete Payment"

✅ **Payment Method Selection**
- Dropdown with options: "UPI" and "Card"
- Default should be "UPI"

✅ **UPI Form Fields** (when UPI selected)
- VPA input field visible
- Placeholder: "user@paytm"

✅ **Card Form Fields** (when Card selected)
- Switch to "Card" in dropdown
- Should show:
  - Card Number field
  - Card Holder field
  - Expiry field (MM/YYYY)
  - CVV field

✅ **Submit Button**
- "Pay Now" button should be visible
- Should be enabled

#### Test Actions:
1. **Test UPI Payment**:
   - Select "UPI" method
   - Enter VPA: `test@paytm`
   - Click "Pay Now"
   - Should show success/error message
   - Check browser console for any errors

2. **Test Card Payment**:
   - Select "Card" method
   - Enter card details:
     - Card Number: `1234567890123456`
     - Card Holder: `John Doe`
     - Expiry: `12/2025`
     - CVV: `123`
   - Click "Pay Now"
   - Should show success/error message

3. **Test Form Validation**:
   - Try submitting empty form
   - Should show validation errors (if implemented)

### 2. Test Embedded Mode
- Open: http://localhost:3001/checkout?order_id=test123&key=key_test_abc123&embedded=true
- This simulates the iframe mode for SDK

#### What to Check:
✅ **Same form appears**
- Form should work the same way
- After payment, should send postMessage to parent window

---

## 🧪 Browser DevTools Checks

### Open DevTools (F12) and Check:

1. **Console Tab**:
   - ✅ No red errors
   - ⚠️ Warnings are acceptable
   - Check for any API call errors

2. **Network Tab**:
   - ✅ API calls to `localhost:8000` should succeed (200 status)
   - ✅ Check if webhook logs API call works
   - ✅ Check if payment creation API call works

3. **Elements Tab** (for Dashboard):
   - ✅ Check for `data-test-id` attributes:
     - `payment-modal`
     - `webhook-config`
     - `webhook-url-input`
     - `webhook-secret`
     - `webhook-logs-table`
     - `api-docs`
     - etc.

---

## 📝 Quick Test Checklist

### Dashboard (http://localhost:3000)
- [ ] Page loads successfully
- [ ] Navigation links work
- [ ] Webhook Configuration page loads
- [ ] Webhook logs table displays data
- [ ] API Documentation page loads
- [ ] Code snippets are readable
- [ ] No console errors

### Checkout (http://localhost:3001)
- [ ] Checkout page loads
- [ ] Payment method dropdown works
- [ ] Form fields switch between UPI/Card
- [ ] Can submit payment form
- [ ] Success/error messages appear
- [ ] No console errors

---

## 🔍 Specific Things to Look For

### ✅ Success Indicators:
- Pages load without blank screens
- Forms are interactive and responsive
- Data displays correctly (webhook logs, etc.)
- Buttons and links are clickable
- No JavaScript errors in console
- API calls succeed (check Network tab)

### ⚠️ Potential Issues to Watch For:
- Blank white screen (check console for errors)
- API calls failing (CORS errors, 404, 500)
- Forms not submitting
- Data not loading (empty tables/lists)
- Buttons not responding
- Styling issues (broken layout)

---

## 🚀 Quick Start Testing

1. **Start with Dashboard**:
   ```
   Open: http://localhost:3000/dashboard/webhooks
   Check: Webhook logs table shows data
   ```

2. **Then Check API Docs**:
   ```
   Open: http://localhost:3000/dashboard/docs
   Check: Code snippets are visible
   ```

3. **Test Checkout**:
   ```
   Open: http://localhost:3001/checkout?order_id=test123&key=key_test_abc123
   Try: Submit a test payment
   ```

---

## 💡 Pro Tips

1. **Use Browser DevTools** (F12) to check:
   - Console for errors
   - Network tab for API calls
   - Elements tab to inspect HTML

2. **Test in Different Browsers**:
   - Chrome/Edge
   - Firefox
   - Safari (if on Mac)

3. **Check Responsive Design**:
   - Resize browser window
   - Test on mobile viewport (DevTools device emulation)

4. **Verify API Integration**:
   - Make sure API is running (http://localhost:8000)
   - Check if API calls in Network tab return 200 status

---

## 🆘 If Something Doesn't Work

1. **Check Service Status**:
   ```powershell
   docker compose ps
   ```
   All services should show "Up"

2. **Check Logs**:
   ```powershell
   docker compose logs dashboard
   docker compose logs checkout
   docker compose logs api
   ```

3. **Check Browser Console**:
   - Open DevTools (F12)
   - Look for error messages
   - Check Network tab for failed requests

4. **Verify URLs**:
   - Dashboard: http://localhost:3000
   - Checkout: http://localhost:3001
   - API: http://localhost:8000/health
