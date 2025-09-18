# MAKINEO - Public deployment notes

This file documents how to deploy the built frontend contained in the `dist/` folder to a static host.

## What is in `dist/`?

When you build the Angular frontend (npm run build), the compiled assets will be placed in `dist/`. This directory is a self-contained static site (HTML/CSS/JS) that can be served by any static host.

## Recommended hosts

- Netlify / Vercel: drop the `dist/` contents or connect the repo and set the build command `npm run build` and publish directory `dist/`.
- GitHub Pages: push `dist/` to the `gh-pages` branch (or use actions to build and publish).
- Nginx: serve `dist/` as the webroot and configure rewrite rules for SPA (redirect all paths to index.html).
- Static object storage (S3 + CloudFront): upload the files and configure cache headers.

## Nginx example configuration

server {
    listen 80;
    server_name example.com;

    root /var/www/makineo/dist;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }

    # Optional: serve API from a different host or proxy
    # location /api/ {
    #     proxy_pass http://api.internal:8000/api/;
    # }
}

## GitHub Pages (simple)

1. Build the frontend locally:

```bash
npm run build
```

2. Use a tool like `gh-pages` to publish `dist/` or create a branch `gh-pages` with the contents of `dist/`.

## Notes

- The application expects an API backend reachable at the routes defined in `proxy.conf.json` during development. In production, ensure the frontend can reach the backend API either by setting correct absolute URLs in environment files or by configuring a reverse proxy.
- SPA routing requires that unknown paths fall back to `index.html` (see Nginx `try_files` above).

## Security

- Do not expose admin or debug endpoints publicly.
- Use HTTPS in production.

---

© MAKINEO
