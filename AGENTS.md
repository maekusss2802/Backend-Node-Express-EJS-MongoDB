# Base44 Dev Environment

## Stack
- **Runtime:** Node.js (Express 4) + EJS views
- **Database:** MongoDB 7 (compose service `mongo`)
- **Entry point:** `bin/www` (was missing from the repo — recreated as the standard express-generator server bootstrap; listens on `PORT` env or 3000)

## Running
```
docker compose -f docker-compose.base44.yml up -d --build
```
The `web` service bind-mounts the source at `/app`, runs `npm install` on startup, then starts `nodemon bin/www` for live reload. Edits to `.js`/`.ejs` files restart the server automatically.

## Key details
- **MongoDB URI** is configurable via `MONGODB_URI` env var (set in compose to `mongodb://mongo:27017/amazonDB`). The original hardcoded `127.0.0.1` fallback remains for local non-Docker use.
- **Session secret** is hardcoded in `app.js` — not an external credential, left as-is.
- No external service credentials are required. All infrastructure (MongoDB) runs locally in compose.
- `node_modules` lives in an anonymous Docker volume so host installs don't interfere.

## Routes
- `/` — home page (sets a session + cookie)
- `/checkCookie`, `/deleteCookie` — cookie demo
- `/checkSession`, `/removeSession` — session demo
- `/create`, `/allUser`, `/delete` — MongoDB CRUD demo (requires MongoDB running)
