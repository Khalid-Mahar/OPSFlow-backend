# OpsFlow Deployment Guide — Railway Backend + Hostinger Frontend

Complete step-by-step guide to deploy your OpsFlow ERP system for production.

---

## 🚀 Phase 1: Railway Backend Deployment

### Step 1: Connect GitHub to Railway

1. Go to **[railway.app](https://railway.app)** and sign up/login
2. Click **New Project** → **Deploy from GitHub**
3. Select the **OPSFlow-backend** repository
4. Railway will auto-detect Node.js and create a service

### Step 2: Add PostgreSQL Database

1. In Railway dashboard, click **+ Create** → **Database** → **PostgreSQL**
2. Railway creates a new PostgreSQL database and provides:
   - `DATABASE_URL` (connection string)
   - Username, password, host, port
3. Copy the `DATABASE_URL` string

### Step 3: Set Environment Variables

1. Go to **Project Settings** → **Variables** (or click the service)
2. Add these environment variables:

```
DATABASE_URL=postgresql://[user]:[password]@[host]:[port]/[database]
JWT_SECRET=generate-strong-random-key-min-32-chars
JWT_REFRESH_SECRET=generate-another-strong-random-key-min-32-chars
JWT_EXPIRES_IN=7d
SUPERADMIN_PASSWORD=YourSecurePassword123!
ENCRYPTION_KEY=32-character-hex-string-for-aes256
PORT=5000
FRONTEND_URL=https://app.yourdomain.com
NODE_ENV=production
```

**Generate strong random keys:**
```bash
# Open Terminal and run:
node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"
# Copy the output and use it for JWT_SECRET
# Do it again for JWT_REFRESH_SECRET and ENCRYPTION_KEY
```

### Step 4: Deploy & Run Database Migrations

1. Railway auto-deploys on push. Check **Deployments** tab
2. Once deployed, click the service and go to **Logs**
3. You should see:
   ```
   Server running on port 5000
   ```

4. **Run Database Seed** (via Railway terminal or webhook):
   ```bash
   # Option A: SSH into Railway and run
   npm run db:seed
   
   # This creates:
   # - Company record
   # - 4 roles (Super Admin, Admin, Team Lead, Employee)
   # - All permissions
   # - Super admin user
   ```

5. **Get Your Backend URL**:
   - Click the service → **Domains** → Copy the Railway-assigned domain
   - Example: `https://opsflow-backend-prod.railway.app`
   - Or use a custom domain if you have one

---

## 🌐 Phase 2: Hostinger Frontend Subdomain Setup

### Step 1: Create Subdomain on Hostinger

1. Login to **Hostinger Dashboard**
2. Go to **Domains** → Select your domain
3. Click **Subdomains** or **DNS Management**
4. Create a new subdomain:
   - **Subdomain**: `app` (result: `app.yourdomain.com`)
   - **Points to**: `public_html/app` (or your Hostinger public directory)

### Step 2: Configure DNS (if needed)

If using a separate host for the subdomain:
1. Get your Hostinger's **A record** (IP address)
2. Add DNS record:
   - **Type**: A
   - **Name**: `app.yourdomain.com`
   - **Value**: Your Hostinger IP
   - Wait 24-48 hours for DNS propagation

### Step 3: SSL Certificate (HTTPS)

1. Most Hostinger plans include **Let's Encrypt SSL**
2. Go to **SSL Certificates** → **Manage**
3. Ensure `app.yourdomain.com` is included in the SSL certificate
4. If not, regenerate the certificate to include the subdomain

---

## 💻 Phase 3: Frontend Build & Deploy to Hostinger

### Step 1: Build Frontend Locally

```bash
cd C:\Users\M Khalid\Downloads\opsflow-erp-v2-fixed\opsflow-v2-clean
# or wherever your frontend repo is

# Update the API URL in frontend code:
# File: frontend/.env.example
# Change: REACT_APP_API_URL=https://your-railway-backend.railway.app/api

# Install dependencies
cd OPSFlow-frontend  # if separate repo
npm install

# Build for production
npm run build
```

This creates a `build/` folder with optimized files.

### Step 2: Compress Build Folder

```bash
# PowerShell
Compress-Archive -Path build -DestinationPath build.zip

# Or use 7-Zip/WinRAR
# Right-click build → Add to archive → build.zip
```

### Step 3: Upload to Hostinger

**Option A: File Manager (Easiest)**

1. Go to **Hostinger File Manager**
2. Navigate to: `public_html/app/` (create `app` folder if needed)
3. Upload `build.zip`
4. Right-click → **Extract**
5. Move all contents of `build/` folder one level up:
   ```
   public_html/app/
   ├── index.html
   ├── static/
   ├── public/
   └── etc...
   ```
6. Delete the `build/` folder and `build.zip`

**Option B: FTP**

```bash
# Using FileZilla or command line FTP:
ftp your-hostinger-ftp-url
# Login with credentials from Hostinger

cd public_html/app
put build.zip
# Extract on server
```

### Step 4: Add .htaccess for React Router

1. In `public_html/app/`, create `.htaccess` file with:

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

This ensures React Router works correctly (SPA routing).

---

## 🔗 Phase 4: Connect Frontend to Backend

### Update Frontend Environment

Before uploading to Hostinger, ensure the frontend points to your Railway backend:

**File: `OPSFlow-frontend/.env`**
```env
REACT_APP_API_URL=https://your-railway-backend.railway.app/api
```

### Rebuild & Redeploy

```bash
npm run build
# Repeat upload steps above
```

---

## ✅ Verification Checklist

- [ ] **Backend on Railway**
  - [ ] Deployments completed successfully
  - [ ] Database migrations ran
  - [ ] Seed data created (company, roles, permissions, super admin)
  - [ ] GET `https://your-railway-backend/api/health` returns 200

- [ ] **Frontend on Hostinger**
  - [ ] Subdomain created (`app.yourdomain.com`)
  - [ ] SSL certificate includes subdomain
  - [ ] Build folder uploaded to `public_html/app`
  - [ ] `.htaccess` file exists for React Router
  - [ ] Can access `https://app.yourdomain.com` in browser

- [ ] **Connectivity**
  - [ ] Frontend loads correctly
  - [ ] Login page appears
  - [ ] Can login with:
    - Email: `muhammad.khalid@opsflow.com`
    - Password: `Admin@2024` (or your set SUPERADMIN_PASSWORD)
  - [ ] Dashboard loads (shows your data)
  - [ ] API requests work (check browser DevTools → Network)

---

## 🐛 Troubleshooting

### Backend Issues

**Problem**: "Application Error" on Railway
- **Solution**: Check **Logs** tab → Look for error messages
- **Common**: Database connection failed → Verify DATABASE_URL

**Problem**: Seed didn't run
- **Solution**: SSH into Railway or run seed via Railway terminal:
  ```bash
  npm run db:seed
  ```

**Problem**: "CORS Error" on frontend
- **Solution**: Update `FRONTEND_URL` in Railway environment to your Hostinger domain:
  ```
  FRONTEND_URL=https://app.yourdomain.com
  ```
  Then redeploy backend

### Frontend Issues

**Problem**: Blank white page
- **Solution**: 
  - Check browser console (F12) for errors
  - Verify `.htaccess` exists and is correct
  - Check `REACT_APP_API_URL` is correct

**Problem**: "Cannot reach API"
- **Solution**:
  - Verify `REACT_APP_API_URL` points to Railway backend
  - Check Railway backend is running (check status on dashboard)
  - Verify CORS is enabled on backend

**Problem**: "Not Found" on subdomain
- **Solution**:
  - Verify subdomain DNS is pointing to Hostinger IP
  - Check files are in correct directory (`public_html/app/`)
  - Verify `.htaccess` file exists

---

## 🚀 Production Optimization

### Backend (Railway)

- Set `NODE_ENV=production`
- Use strong, random JWT secrets
- Enable database backups (Railway → Database → Backups)
- Monitor logs for errors
- Set up error alerting (Railway → Monitoring)

### Frontend (Hostinger)

- Enable **Gzip compression** (usually default)
- Use **CDN** if available
- Enable **Browser caching** for static files
- Minimize JS/CSS bundle size
- Monitor performance with Chrome DevTools

---

## 📧 Support & Monitoring

### Railway Monitoring
- Check **Metrics** tab for CPU, memory, network usage
- Set up **alerts** for service failures
- Enable **log streaming** for debugging

### Hostinger Monitoring
- Check **Site Health** in dashboard
- Monitor **storage usage**
- Keep backups enabled

---

## 🎯 Next Steps (Optional)

1. **Custom Domain for Backend**:
   - Point custom domain to Railway (CNAME record)
   - Update `FRONTEND_URL` in Railway

2. **Database Backups**:
   - Set up automatic PostgreSQL backups
   - Test restoration

3. **Analytics**:
   - Add Google Analytics to frontend
   - Monitor user behavior

4. **CI/CD Pipeline**:
   - Railway auto-deploys on git push
   - Consider GitHub Actions for frontend

---

## 📞 Quick Reference

| Component | Platform | URL | 
|-----------|----------|-----|
| Backend API | Railway | `https://opsflow-api.railway.app` |
| Frontend App | Hostinger | `https://app.yourdomain.com` |
| Database | Railway PostgreSQL | Managed by Railway |

---

**Deployment Date**: 2026-06-04  
**Status**: Ready for Production ✅
