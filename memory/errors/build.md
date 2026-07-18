# Build & Serve Errors

See [[../../main.md]] section 2 for the logging protocol.

---

### [ERROR] Frontend changes not visible — stale dist/ build
**Context:** Vite app with both `src/` (dev) and `dist/` (built). App opened via the built `dist/index.html` or a static server pointing at `dist/`.
**Symptom:** Edits to `src/main.js` / `src/style.css` don't appear in the running app; user reports "fix didn't work" or missing features (e.g. avatar not showing, old layout).
**Root cause:** `dist/` is a static build from an earlier `src/` state. The dev server (`vite`, default :5173) serves live `src/`, but if the built bundle is what's actually being served, source changes are invisible until rebuild.
**Solution:** Run `npm run build` in the app folder to regenerate `dist/`. Verify the new hashed asset filenames changed (e.g. `dist/assets/index-*.js`). If a dev server is running, confirm which port is actually being used.
**Tags:** vite, dist, build, stale, frontend, cache, static

---

### [ERROR] API returns empty fields after code edits (server process stale)
**Context:** Node API server started in a previous session; source `server.js` later edited (e.g. added `xp`/`level` to `GET /users`).
**Symptom:** Endpoint returns old/missing fields (`xp= level=` empty) even though the source now emits them.
**Root cause:** The running node process loaded the old `server.js` at startup; it keeps serving stale code until restarted.
**Solution:** Find the listening PID (`Get-NetTCPConnection -LocalPort <port>`), `Stop-Process`, then restart `node server.js`. Re-verify the endpoint output.
**Tags:** node, server, stale, restart, api, port
