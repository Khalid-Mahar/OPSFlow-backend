# OpsFlow Quick Reference — Commands & URLs

## 🚀 Quick Deploy Commands

### Generate Strong Random Keys (for Railway env vars)
```bash
# Open terminal and run 3 times (for JWT_SECRET, JWT_REFRESH_SECRET, ENCRYPTION_KEY)
node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"
```

### Build Frontend Locally
```bash
cd OPSFlow-frontend
npm install
npm run build
```

### Compress for Upload
```bash
# PowerShell
Compress-Archive -Path build -DestinationPath build.zip
```

---

## 🔧 Railway Setup

### Environment Variables Template
```
DATABASE_URL=postgresql://[user]:[pass]@[host]:[port]/[db]
JWT_SECRET=[32+ random chars]
JWT_REFRESH_SECRET=[32+ random chars]
JWT_EXPIRES_IN=7d
SUPERADMIN_PASSWORD=[strong password]
ENCRYPTION_KEY=[32-char hex]
PORT=5000
FRONTEND_URL=https://app.yourdomain.com
NODE_ENV=production
```

### Database Seed Command (if needed)
```bash
npm run db:seed
```

### Railway Domains
- Check **Project** → **Settings** → **Domains**
- Your backend will be at: `https://[project-name].railway.app`

---

## 🌐 Hostinger Setup

### Subdomain Path
- **Public HTML Path**: `/public_html/app/`
- **Access URL**: `https://app.yourdomain.com`

### .htaccess Content
```apache
<IfModule mod_rewrite.c>
  RewriteEngine On
  RewriteBase /app/
  RewriteRule ^index\.html$ - [L]
  RewriteCond %{REQUEST_FILENAME} !-f
  RewriteCond %{REQUEST_FILENAME} !-d
  RewriteRule . /app/index.html [L]
</IfModule>
```

### File Upload Path
```
public_html/app/
├── index.html
├── static/
│   ├── js/
│   ├── css/
│   └── media/
├── favicon.ico
└── .htaccess
```

---

## 🔗 API URLs

### Local Development
```
Backend: http://localhost:5000/api
Frontend: http://localhost:3000
```

### Production (after deployment)
```
Backend: https://your-railway-backend.railway.app/api
Frontend: https://app.yourdomain.com
```

---

## 👤 Default Login Credentials

```
Email: muhammad.khalid@opsflow.com
Password: Admin@2024 (or your SUPERADMIN_PASSWORD)
Role: Super Admin
```

---

## 🔑 Important Files

| File | Location | Purpose |
|------|----------|---------|
| `.env` | `backend/` | Backend environment config (DO NOT commit) |
| `.env.example` | Both repos | Template for environment variables |
| `package.json` | Both repos | Dependencies & scripts |
| `prisma/schema.prisma` | `backend/` | Database schema |
| `.htaccess` | `public_html/app/` | React Router SPA config |
| `build/` | Frontend | Production-ready optimized code |

---

## 📞 Support URLs

| Service | URL | Purpose |
|---------|-----|---------|
| Railway Docs | https://docs.railway.app | Backend deployment help |
| Hostinger Support | https://support.hostinger.com | Hosting issues |
| React Docs | https://react.dev | Frontend framework |
| Prisma Docs | https://www.prisma.io/docs | Database ORM |

---

## ⚠️ Critical Don'ts

- ❌ Don't commit `.env` file to GitHub
- ❌ Don't expose database credentials in code
- ❌ Don't use weak passwords for super admin
- ❌ Don't delete PostgreSQL database in production without backup
- ❌ Don't reload page during database migrations
- ❌ Don't forget to update `FRONTEND_URL` in Railway

---

## ✅ Final Checklist

- [ ] Backend on Railway (deployed & running)
- [ ] PostgreSQL database created & seeded
- [ ] Subdomain created on Hostinger
- [ ] SSL certificate includes subdomain
- [ ] Frontend built locally
- [ ] Frontend uploaded to Hostinger
- [ ] `.htaccess` file in place
- [ ] `REACT_APP_API_URL` points to Railway backend
- [ ] Can login with super admin credentials
- [ ] Dashboard loads without errors
- [ ] All modules accessible based on role

---

**Last Updated**: 2026-06-04  
**Version**: Production Ready v1.0
