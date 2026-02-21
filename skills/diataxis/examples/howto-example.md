---
diataxis_type: how-to
diataxis_goal: Add rate limiting to an Express API
---

# How to Add Rate Limiting to Your API

## Overview

This guide shows you how to add rate limiting to an Express API to prevent abuse and ensure fair usage.

## Prerequisites

- An existing Express application
- npm or yarn package manager

## Steps

### 1. Install the rate limiting package

```bash
npm install express-rate-limit
```

### 2. Configure the rate limiter

Add the rate limiter to your application entry point:

```javascript
const rateLimit = require('express-rate-limit');

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000,
  max: 100,
  standardHeaders: true,
  legacyHeaders: false,
});

app.use(limiter);
```

This configuration allows 100 requests per 15-minute window per IP address.

### 3. Apply rate limiting to specific routes

If you need different limits for specific routes:

```javascript
const authLimiter = rateLimit({
  windowMs: 15 * 60 * 1000,
  max: 5,
});

app.use('/api/login', authLimiter);
```

### 4. Handle rate limit responses

When a client exceeds the limit, they receive a 429 status code. To customize the response:

```javascript
const limiter = rateLimit({
  windowMs: 15 * 60 * 1000,
  max: 100,
  message: { error: 'Too many requests. Please try again later.' },
});
```

## Verification

Send requests rapidly to confirm rate limiting is active:

```bash
for i in $(seq 1 110); do
  curl -s -o /dev/null -w "%{http_code}\n" http://localhost:3000/api/tasks
done
```

After 100 requests, you should see `429` status codes.

## Troubleshooting

- **Rate limiting not applied:** Ensure `app.use(limiter)` appears before your route definitions.
- **Wrong IP detected behind a proxy:** Set `app.set('trust proxy', 1)` before configuring the limiter.

## Related

- [express-rate-limit configuration reference](../reference/rate-limit-options.md)
- [Understanding API security patterns](../explanation/api-security.md)
