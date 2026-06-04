# OpsFlow Deployment Checklist

## Railway Backend Setup

### Database
- [ ] Created PostgreSQL database on Railway
- [ ] Copied `DATABASE_URL` from Railway dashboard
- [ ] Connection string working (tested locally if needed)

### Environment Variables (Railway Dashboard)
- [ ] `DATABASE_URL` set
- [ ] `JWT_SECRET` set (strong random key, 32+ chars)
- [ ] `JWT_REFRESH_SECRET` set (strong random key, 32+ chars)
- [ ] `JWT_EXPIRES_IN=7d`
- [ ] `SUPERADMIN_PASSWORD` set
- [ ] `ENCRYPTION_KEY` set (32-char hex)
- [ ] `PORT=5000`
- [ ] `FRONTEND_URL=https://app.yourdomain.com` (update with your domain)
- [ ] `NODE_ENV=production`

### Backend Deployment
- [ ] Repository connected to Railway
- [ ] Deployment completed successfully
- [ ] Logs show "Server running on port 5000"
- [ ] Database migrations applied
- [ ] Seed script executed (`npm run db:seed`)
  - [ ] Super admin created: `muhammad.khalid@opsflow.com`
  - [ ] 4 roles created: Super Admin, Admin, Team Lead, Employee
  - [ ] Permissions created
- [ ] Backend URL obtained (e.g., `https://opsflow-backend.railway.app`)

### Backend Testing
- [ ] Backend health check: `curl https://your-backend-url/api/health`
- [ ] Can access `/api/auth/login` endpoint
- [ ] JWT tokens are being issued on login

---

## Hostinger Frontend Setup

### Domain & Subdomain
- [ ] Have Hostinger account with active domain
- [ ] Created subdomain `app.yourdomain.com`
- [ ] DNS propagated (wait 24-48 hours if just created)
- [ ] Subdomain pointing to Hostinger IP/public_html

### SSL Certificate
- [ ] SSL certificate generated/updated for subdomain
- [ ] HTTPS working on `https://app.yourdomain.com`
- [ ] No SSL warnings in browser

### Directory Structure
- [ ] Created `/app` folder in `public_html` directory
- [ ] `.htaccess` file created with React Router rules
- [ ] Static files will be served from `public_html/app/`

---

## Frontend Build & Upload

### Local Build
- [ ] Updated `REACT_APP_API_URL` to point to Railway backend
  ```
  REACT_APP_API_URL=https://your-railway-backend.railway.app/api
  ```
- [ ] Ran `npm install`
- [ ] Ran `npm run build`
- [ ] `build/` folder created successfully
- [ ] Compressed `build/` to `build.zip`

### Upload to Hostinger
- [ ] Uploaded `build.zip` to `public_html/app/`
- [ ] Extracted ZIP file
- [ ] Verified files:
  - [ ] `index.html` in `public_html/app/`
  - [ ] `static/` folder in `public_html/app/`
  - [ ] Other build artifacts present
- [ ] Deleted `build.zip` and original `build/` folder

### .htaccess Configuration
- [ ] Created `.htaccess` file in `public_html/app/`
- [ ] Content matches React Router requirements:
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

---

## Production Testing

### Connectivity
- [ ] Can access `https://app.yourdomain.com` in browser
- [ ] Login page loads without errors
- [ ] Can see input fields: email, password, login button

### Authentication
- [ ] Can login with:
  - Email: `muhammad.khalid@opsflow.com`
  - Password: `Admin@2024` (or your SUPERADMIN_PASSWORD)
- [ ] Redirects to dashboard after successful login
- [ ] Bearer token appears in browser Network tab (Authorization header)

### API Connectivity
- [ ] Dashboard loads user data
- [ ] Network requests show 200/201 responses
- [ ] No 401/403 errors in console
- [ ] No CORS errors in console

### Features
- [ ] Can navigate between pages
- [ ] Sidebar menu displays correctly
- [ ] Can view dashboard stats
- [ ] Permissions-based UI works (some features visible/hidden based on role)

---

## Performance & Security

### Performance
- [ ] Page load time < 3 seconds
- [ ] No JavaScript errors in browser console
- [ ] Images/static files loading from CDN (if configured)

### Security
- [ ] HTTPS enforced on all pages
- [ ] No sensitive data in logs
- [ ] JWT tokens stored securely (in memory, not localStorage)
- [ ] CORS properly configured

---

## Monitoring & Backups

### Railway Backend
- [ ] Enabled automated backups for PostgreSQL
- [ ] Set up error alerts/monitoring
- [ ] Checked resource usage (CPU, memory)

### Hostinger Frontend
- [ ] Enabled website backups (if available)
- [ ] Verified uptime monitoring

---

## Troubleshooting Log

**Issues Encountered**:
- [ ] None yet

**Resolutions Applied**:
- [ ] N/A

---

## Go-Live Status

- [ ] All items checked
- [ ] Production deployment complete ✅
- [ ] Users can access system ✅
- [ ] Data is being persisted ✅
- [ ] Ready for user training

---

**Deployment Date**: _______________  
**Deployed By**: _______________  
**Notes**: _______________
