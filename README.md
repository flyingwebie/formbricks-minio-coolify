# Formbricks + MinIO for Coolify

Deploy [Formbricks](https://formbricks.com) with self-hosted S3 storage (MinIO) on [Coolify](https://coolify.io).

## 📺 Video Tutorial

[![Formbricks + MinIO Setup](https://img.youtube.com/vi/ZrvlLKe4TTA/maxresdefault.jpg)](https://youtu.be/ZrvlLKe4TTA)

---

## Quick Start

### 1. Create Service in Coolify

Create a new **Docker Compose** service and paste `docker-compose.yaml`.

### 2. Configure Domains

| Service | Port | Variable |
|---------|------|----------|
| formbricks | 3000 | `SERVICE_URL_FORMBRICKS` |
| minio | 9000 | `MINIO_SERVER_URL` (S3 API) |
| minio | 9001 | `MINIO_BROWSER_REDIRECT_URL` (Console) |

### 3. Set Environment Variables

```env
# MinIO URLs
MINIO_SERVER_URL=https://files.yourdomain.com
MINIO_BROWSER_REDIRECT_URL=https://minio.yourdomain.com

# SMTP (required)
MAIL_FROM=noreply@yourdomain.com
SMTP_HOST=smtp.resend.com
SMTP_PORT=465
SMTP_USER=resend
SMTP_PASSWORD=re_xxxxxxxxxxxx
SMTP_SECURE_ENABLED=1
```

### 4. Deploy

Click **Deploy** in Coolify.

### 5. ⚠️ Create MinIO Bucket (Required!)

> **File uploads won't work without this step!**

1. Open MinIO Console (`MINIO_BROWSER_REDIRECT_URL`)
2. Login with `SERVICE_USER_MINIO` / `SERVICE_PASSWORD_MINIO` (find in Coolify)
3. **Buckets** → **Create Bucket** → Name: `formbricks-uploads`

**Bucket naming:** Use only lowercase, numbers, hyphens. No `_`, `&`, `%`.

### 6. Access Formbricks

Open your Formbricks URL and complete setup.

---

## Services

| Service | Image |
|---------|-------|
| formbricks | `ghcr.io/formbricks/formbricks:latest` |
| postgresql | `pgvector/pgvector:pg16` |
| valkey | `valkey/valkey:8-alpine` |
| minio | `ghcr.io/coollabsio/minio:RELEASE.2025-10-15T17-29-55Z` |

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| File uploads fail | Create bucket `formbricks-uploads` in MinIO Console |
| CORS errors | Set `MINIO_API_CORS_ALLOW_ORIGIN=*` |
| Invalid URL errors | Ensure URLs include `https://` |
| Redis required | Check Valkey container is running |

---

## Resources

- [Formbricks Docs](https://formbricks.com/docs)
- [Coolify Docs](https://coolify.io/docs)
- [MinIO Docs](https://min.io/docs/minio/)