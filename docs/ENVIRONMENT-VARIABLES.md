# Environment Variables & Configuration

[Back to Index](README.md)

---

## Overview

The Strapi CMS relies on environment variables for all external connections (database, storage, caching, video, search, monitoring). This document is the single reference for understanding what each variable does and which ones matter for frontend development.

---

## Environments

| Environment | Strapi Admin | API Base URL |
|-------------|-------------|-------------|
| **Production** | `https://prod-strapi.jkyog.org/admin` | `https://prod-strapi.jkyog.org/api/` |
| **Staging** | `https://jkyog-strapi-staging.up.railway.app/admin` | `https://jkyog-strapi-staging.up.railway.app/api/` |

> Always test on **Staging** before making production changes. Staging and Production are separate instances with separate databases.

---

## Frontend-Relevant Variables

These are the variables a frontend developer needs to know about. You don't set them directly -- they're configured on the Strapi server -- but you need to know what URLs and services they point to.

| Variable | What It Controls | Frontend Impact |
|----------|-----------------|----------------|
| `PUBLIC_URL` | The public-facing Strapi URL | This is the API base URL your frontend calls (e.g., `https://prod-strapi.jkyog.org`) |
| `CDN_URL` | CloudFront CDN domain | All media URLs (images, files) are served from this domain. Use it in CSP headers and image loading. |
| `CDN_ROOT_PATH` | S3 path prefix | Prefixed to media file paths. Media URLs follow the pattern: `https://{CDN_URL}/{CDN_ROOT_PATH}/{filename}` |
| `GOOGLE_MAPS_API_KEY` | Google Maps API key | Used by the venue field in events. If the frontend renders maps, it needs its own key. |
| `AWS_SIGNED_URL_EXPIRES` | Signed URL TTL (default: 900s) | Private media files have signed URLs that expire after 15 minutes. Re-fetch if expired. |

### CSP (Content Security Policy) Domains

If your frontend sets CSP headers, whitelist these domains for media loading:

| Domain | Purpose |
|--------|---------|
| `https://{CDN_URL}` | CloudFront CDN -- all images and files |
| `https://stream.mux.com` | Mux video streaming |
| `https://image.mux.com` | Mux video thumbnails |
| `https://maps.googleapis.com` | Google Maps (venue fields) |

---

## Complete Environment Variable Reference

### Application

| Variable | Default | Required | Purpose |
|----------|---------|----------|---------|
| `HOST` | `0.0.0.0` | Yes | Server bind host |
| `PORT` | `1337` | Yes | Server port |
| `PUBLIC_URL` | `http://localhost:1337` | Yes | Public-facing URL (API base) |
| `APP_KEYS` | -- | Yes | Application encryption keys (comma-separated) |
| `API_TOKEN_SALT` | -- | Yes | API token hashing salt |
| `ADMIN_JWT_SECRET` | -- | Yes | Admin JWT signing secret |
| `JWT_SECRET` | -- | Yes | User JWT signing secret |
| `TRANSFER_TOKEN_SALT` | -- | Yes | Transfer token hashing salt |

### Database

> **Backend only.** Frontend developers do not need to configure these.

| Variable | Default | Required | Purpose |
|----------|---------|----------|---------|
| `DATABASE_CLIENT` | `postgres` | Yes | Database client (`postgres` or `sqlite`) |
| `DATABASE_HOST` | `postgres` | Yes | Database hostname |
| `DATABASE_PORT` | `5432` | Yes | Database port |
| `DATABASE_NAME` | `jkyogi` | Yes | Database name |
| `DATABASE_USERNAME` | `postgres` | Yes | Database user |
| `DATABASE_PASSWORD` | `admin` | Yes | Database password |
| `DATABASE_URL` | -- | No | Full connection string (alternative to individual vars) |
| `DATABASE_SCHEMA` | `public` | No | PostgreSQL schema |
| `DATABASE_SSL` | `false` | No | Enable SSL |
| `DATABASE_POOL_MIN` | `0` | No | Minimum connections |
| `DATABASE_POOL_MAX` | `3` | No | Maximum connections |

### AWS S3 & CloudFront (Media Storage)

| Variable | Default | Required | Purpose |
|----------|---------|----------|---------|
| `AWS_ACCESS_KEY_ID` | -- | Yes | AWS credentials |
| `AWS_ACCESS_SECRET` | -- | Yes | AWS secret key |
| `AWS_REGION` | -- | Yes | S3 bucket region |
| `AWS_BUCKET` | -- | Yes | S3 bucket name |
| `CDN_URL` | -- | Yes | CloudFront CDN domain (media served from here) |
| `CDN_ROOT_PATH` | -- | No | S3 path prefix |
| `AWS_ACL` | `public-read` | No | S3 object ACL |
| `AWS_SIGNED_URL_EXPIRES` | `900` | No | Signed URL TTL in seconds (15 min) |

### Redis (API Caching)

> **Backend only.** These control the Redis cache that affects API response freshness.

| Variable | Default | Required | Purpose |
|----------|---------|----------|---------|
| `REDIS_HOST` | `localhost` | No | Redis hostname |
| `REDIS_PORT` | `6379` | No | Redis port |
| `REDIS_PASSWORD` | -- | No | Redis password |
| `REDIS_DB` | `0` | No | Redis database number |

### External Services

| Variable | Default | Required | Purpose |
|----------|---------|----------|---------|
| `GOOGLE_MAPS_API_KEY` | -- | Yes | Google Maps for venue fields |
| `MUX_TOKEN_ID` | -- | Yes | Mux video API token ID |
| `MUX_TOKEN_SECRET` | -- | Yes | Mux video API token secret |
| `SENTRY_DSN` | -- | No | Sentry error tracking DSN |

### Webhooks

| Variable | Default | Required | Purpose |
|----------|---------|----------|---------|
| `WEBHOOKS_POPULATE_RELATIONS` | `false` | No | Include relations in webhook payloads |

### Supabase (Optional)

| Variable | Default | Required | Purpose |
|----------|---------|----------|---------|
| `SUPABASE_SERVICE_URL` | -- | No | Supabase service URL |
| `SUPABASE_SERVICE_API_TOKEN` | -- | No | Supabase API token |

---

## Quick Reference: "Where Does This URL Come From?"

| URL You See | Source Variable | Example |
|------------|----------------|---------|
| API base URL (`/api/appevents`) | `PUBLIC_URL` | `https://prod-strapi.jkyog.org` |
| Image/file URLs in API responses | `CDN_URL` + `CDN_ROOT_PATH` | `https://d2xyz.cloudfront.net/uploads/image.png` |
| Video playback URLs | Mux (`MUX_TOKEN_ID`) | `https://stream.mux.com/{playbackId}.m3u8` |
| Video thumbnail URLs | Mux | `https://image.mux.com/{playbackId}/thumbnail.png` |
| Map embeds in venue fields | `GOOGLE_MAPS_API_KEY` | `https://maps.googleapis.com/...` |

---

## Related Docs

- [Data & API Reference](DATA.md) -- API endpoints, caching, query patterns
- [Architecture](ARCHITECTURE.md) -- system overview, tech stack
- [Content Model](MODEL.md) -- content types, fields, components
