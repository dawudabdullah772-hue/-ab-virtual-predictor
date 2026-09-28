# AB Virtual Predictor v23 — Web Hosting Guide

This release is prepared for hosting as a normal web service. It listens on `0.0.0.0` and uses the platform-provided `PORT` environment variable. Persistent SQLite data goes to `DATA_DIR` (default `./data`).

## Easiest: Render

1. Put this project in a GitHub repository.
2. In Render, create a **Blueprint** from the repository.
3. Render will detect `render.yaml` and build the Docker service.
4. The included configuration uses `/data` for the SQLite database and a persistent disk. If your Render plan does not support persistent disks, upgrade the service or use another persistent database/storage option before relying on stored history.
5. After deployment, open the generated `https://...onrender.com` address.
6. Confirm `/api/health` returns JSON with `ok: true`.

## Railway

1. Push the project to GitHub.
2. Create a new Railway project from the repository.
3. Railway will use the included `Dockerfile`/`railway.json`.
4. Add a persistent volume mounted at `/data` if you want SQLite history to survive redeployments.
5. Open the generated public domain.

## Generic Docker hosting

Build:

    docker build -t ab-virtual-predictor .

Run:

    docker run -d --name ab-predictor -p 8000:8000 -v ab-predictor-data:/data ab-virtual-predictor

Then visit the host's public address. Do not expose a development-only localhost address to users.

## Feed configuration

The provider connectors are disabled by default. Set:

    SPORTYBET_ENABLED=true
    SPORTYBET_VIRTUAL_ENABLED=true
    SPORTYBET_REGION=gh

Only enable the connectors if the provider pages/endpoints are permitted and accessible from your hosting environment. The connector is read-only and does not log in, stake, place bets, or handle payment credentials.

## Important SQLite note

A normal container filesystem may be destroyed during redeploys/restarts. Use a persistent volume/disk or move the database to a managed database before treating the deployment as a permanent production system.

## Security note

The app is a research dashboard, not an authentication system. Before exposing it publicly to untrusted users, put it behind platform authentication/access control or add proper application authentication and rate limiting.
