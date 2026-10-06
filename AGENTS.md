# Base44 Dev Environment

This is a Node.js + Express + EJS + MongoDB tutorial app.

## Running

```
docker compose -f docker-compose.base44.yml up -d --build
```

The app is served on host port 3000. Health check: `GET /`.

## Architecture notes

- **No `app.listen` in `app.js`.** `app.js` only exports the Express app. The
  server is started by `bin/www` (referenced by `package.json`'s `start`
  script), which was missing from the original repo and was added for this
  environment.
- **No `views/` directory in the original repo.** `app.js` sets the EJS view
  engine and routes call `res.render('index')` / `res.render('error')`, so
  `views/index.ejs` and `views/error.ejs` were added to make rendering work.
- **MongoDB URI is configurable.** `routes/users.js` connects with
  `process.env.MONGODB_URI || "mongodb://127.0.0.1:27017/amazonDB"`. The
  compose service sets `MONGODB_URI=mongodb://mongo:27017/amazonDB`; the
  fallback preserves the original local-run behavior.
- MongoDB runs as a `mongo:7` compose service. The app depends on it being
  healthy before starting.
- Live reload: the web service runs `node --watch bin/www` (Node 22 built-in
  file watching) with the source bind-mounted, so edits appear without
  rebuilding the image.
- No external credentials are required. The session secret in `app.js` is a
  hardcoded dev placeholder (not a managed secret).
