# Implementation Notes

## Project Structure

This is a production-ready payment gateway implementation with the following components:

### Backend (Node.js/Express)
- **Location**: `backend/`
- **API Server**: `backend/src/server.js`
- **Worker Service**: `backend/src/worker.js`
- **Job Queues**: Bull (Redis-based)
- **Database**: PostgreSQL

### Frontend
- **Dashboard**: `dashboard/` (React app)
- **Checkout Service**: `checkout/` (Express server serving checkout page and SDK)

### SDK
- **Location**: `checkout-widget/`
- **Build Output**: `checkout-widget/dist/checkout.js`

## Key Features Implemented

1. ✅ Asynchronous payment processing using Redis + Bull
2. ✅ Webhook delivery with HMAC signature verification
3. ✅ Exponential backoff retry mechanism (5 attempts)
4. ✅ Embeddable JavaScript SDK
5. ✅ Refund management (full and partial)
6. ✅ Idempotency keys for payment creation
7. ✅ Enhanced dashboard with webhook configuration
8. ✅ API documentation page

## Database Schema

All tables are created with proper indexes:
- `merchants` (with `webhook_secret` column)
- `payments` (with `captured` column)
- `refunds`
- `webhook_logs`
- `idempotency_keys`

## Environment Variables

### Backend/Worker
- `DATABASE_URL`: PostgreSQL connection string
- `REDIS_URL`: Redis connection string
- `TEST_MODE`: Enable test mode (true/false)
- `TEST_PROCESSING_DELAY`: Processing delay in ms (default: 1000)
- `TEST_PAYMENT_SUCCESS`: Payment success flag (default: true)
- `WEBHOOK_RETRY_INTERVALS_TEST`: Use test retry intervals (true/false)

## API Endpoints

### Payments
- `POST /api/v1/payments` - Create payment (async)
- `POST /api/v1/payments/:id/capture` - Capture payment
- `POST /api/v1/payments/:id/refunds` - Create refund

### Refunds
- `GET /api/v1/refunds/:id` - Get refund details

### Webhooks
- `GET /api/v1/webhooks` - List webhook logs
- `POST /api/v1/webhooks/:id/retry` - Retry webhook

### Test
- `GET /api/v1/test/jobs/status` - Job queue status

## Building and Running

1. Build SDK (optional, if serving from checkout service):
   ```bash
   cd checkout-widget
   npm install
   npm run build
   cp dist/checkout.js ../checkout/public/checkout.js
   ```

2. Start all services:
   ```bash
   docker-compose up -d
   ```

3. Run database migrations (if needed):
   ```bash
   docker-compose exec api node backend/src/db/migrate.js
   ```

## Notes

- The checkout service expects the SDK to be built and placed in `checkout/public/checkout.js`
- Test merchant credentials: `key_test_abc123` / `secret_test_xyz789`
- Webhook secret for test merchant: `whsec_test_abc123`
