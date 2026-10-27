# EventHub booking-management tests

This folder is a standalone Playwright project for the EventHub booking scenarios. It keeps the test code, configuration, and run instructions together. The suite uses the EventHub UI for user journeys and the documented API for isolated event setup, persisted-state assertions, inventory races, and cleanup.

## Requirements

- Node.js 20 or newer
- A dedicated EventHub sandbox and test accounts
- Access to the EventHub app and API URLs

Never use production data or real customer accounts. The suite creates and deletes events and bookings. It refuses to run mutating cases unless `EVENTHUB_ALLOW_MUTATIONS=1` is set.

## Configure

Set these variables in the shell or as protected GitHub Actions secrets/variables. Do not commit credentials or a `.env` file.

| Variable | Required | Purpose |
|---|---:|---|
| `EVENTHUB_BASE_URL` | No | App URL; defaults to the public EventHub app URL. |
| `EVENTHUB_API_URL` | Yes | API base URL including `/api`, for example `https://your-sandbox.example/api`. |
| `EVENTHUB_ALLOW_MUTATIONS` | Yes | Must be `1` to enable tests that create bookings/events. |
| `EVENTHUB_EMAIL` | Yes | Dedicated primary test-account email. |
| `EVENTHUB_PASSWORD` | Yes | Dedicated primary test-account password. |
| `EVENTHUB_SECOND_EMAIL` | BKG-08 | Dedicated second-account email for ownership isolation. |
| `EVENTHUB_SECOND_PASSWORD` | BKG-08 | Dedicated second-account password. |
| `EVENTHUB_BOOKING_LIMIT` | BKG-15 | Configured limit; use a fresh dedicated account for this case. |
| `EVENTHUB_LIMIT_EMAIL` | BKG-15 | Fresh account email used only for the booking-limit scenario. |
| `EVENTHUB_LIMIT_PASSWORD` | BKG-15 | Fresh account password used only for the booking-limit scenario. |

Tests BKG-05 and BKG-14 create a one-seat event and consume its last seat through the API. BKG-15 is skipped unless the configured limit is supplied. The product does not document the limit or cancellation inventory policy, so the suite does not assume either.

## Install and run

```powershell
npm ci
npx playwright install chromium
npm test
```

List tests without running them:

```powershell
npm run test:list
```

Only signed-out access checks run without credentials and the explicit mutation opt-in; authenticated cases are reported as skipped with the missing configuration explained.

## GitHub

Add this folder to the target repository. To run it in GitHub Actions, configure the variables/secrets above and place a workflow under the repository-root `.github/workflows/` directory. The current workspace has no GitHub remote, so it has not been pushed.
# Eventhub-booking
