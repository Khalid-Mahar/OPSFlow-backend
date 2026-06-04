# OpsFlow ERP v2 — Backend API

A production-ready Enterprise Resource Planning (ERP) backend built with **Node.js**, **Express**, **TypeScript**, **PostgreSQL**, and **Prisma**.

---

## 🚀 Features

- **JWT Authentication** with refresh tokens
- **Role-Based Access Control (RBAC)** with 4 core roles (Super Admin, Admin, Team Lead, Employee)
- **AES-256 Credential Vault** for secure credential storage
- **Audit Logging** with immutable transaction history
- **File Management** with upload/download support
- **Real-time Notifications** system
- **15+ Business Modules**: Users, Employees, Assets, SIMs, Projects, Tasks, Todos, Files, Notifications, Audit, Dashboard, Tickets, Credentials, Credentials Management, and more
- **Fully Typed** with TypeScript
- **Pagination, Filtering, Sorting** across all endpoints
- **Error Handling** with structured error middleware
- **Request Logging** for debugging and monitoring

---

## 📋 Prerequisites

- **Node.js** ≥ 18
- **PostgreSQL** ≥ 14
- **npm** or **yarn**

---

## 🛠️ Installation

### 1. Clone the Repository

```bash
git clone https://github.com/Khalid-Mahar/OPSFlow-backend.git
cd OPSFlow-backend
```

### 2. Install Dependencies

```bash
npm install
```

### 3. Configure Environment Variables

Create a `.env` file in the root directory:

```env
DATABASE_URL="postgresql://username:password@localhost:5432/opsflow_v2"
JWT_SECRET="your-super-secret-jwt-key-min-32-characters-long"
JWT_REFRESH_SECRET="your-refresh-secret-key-min-32-characters-long"
JWT_EXPIRES_IN="7d"
SUPERADMIN_PASSWORD="YourSecurePassword123!"
ENCRYPTION_KEY="32-character-aes-encryption-key!"
PORT=5000
FRONTEND_URL="http://localhost:3000"
NODE_ENV="development"
```

> ⚠️ **For Production**: Update all secret keys, set `NODE_ENV="production"`, use a secure database URL, and enable SSL for PostgreSQL.

### 4. Setup Database

```bash
# Run migrations
npx prisma migrate deploy

# Seed with production data (creates super admin, roles, permissions)
npm run db:seed
```

After seeding, you'll see:
```
✅ Super Admin created: muhammad.khalid@opsflow.com
📋 LOGIN CREDENTIALS:
   Email: muhammad.khalid@opsflow.com
   Password: Admin@2024
   Role: Super Admin (full access)
```

---

## 🎯 Available Scripts

```bash
# Development server (with auto-reload)
npm run dev

# Build TypeScript
npm run build

# Start production server
npm start

# Run database migrations
npx prisma migrate dev

# Seed database with production data
npm run db:seed

# Open Prisma Studio (database GUI)
npx prisma studio

# Run TypeScript type checking
npx tsc --noEmit
```

---

## 📡 API Endpoints

### Authentication

- `POST /api/auth/login` — Login with email/password
- `POST /api/auth/refresh` — Refresh JWT token
- `POST /api/auth/logout` — Logout

### Core Modules

- `/api/users` — User management
- `/api/employees` — Employee profiles & management
- `/api/assets` — IT asset tracking
- `/api/sims` — SIM & camera management
- `/api/credentials` — Encrypted credential vault
- `/api/projects` — Project lifecycle management
- `/api/tasks` — Kanban-style task system
- `/api/todos` — Daily operations checklist
- `/api/files` — File upload & storage
- `/api/notifications` — In-app notifications
- `/api/audit` — Immutable audit logs
- `/api/dashboard` — Statistics & charts
- `/api/tickets` — Support ticket system

All endpoints support:
- **Pagination**: `?page=1&limit=10`
- **Sorting**: `?sort=createdAt&order=desc`
- **Filtering**: `?status=ACTIVE&role=Admin`

---

## 🔐 Database Schema

Key entities:
- **Companies** — Multi-tenant support
- **Users** — Authentication & roles
- **Roles & Permissions** — RBAC system
- **Employees** — HR data
- **Assets** — IT asset tracking
- **Projects** — Work organization
- **Tasks** — Kanban board data
- **Credentials** — AES-256 encrypted secrets
- **Audit Logs** — Immutable change history
- **Notifications** — Real-time alerts
- **Files** — File management

View full schema: `prisma/schema.prisma`

---

## 🚢 Deployment

### Railway (Recommended)

1. Connect your GitHub repo to Railway
2. Set environment variables in Railway dashboard
3. Railway auto-detects Node.js and installs dependencies
4. Database migrations run automatically on deployment
5. Your API is live!

### Environment Variables for Production

```env
DATABASE_URL="postgresql://user:pass@railway-db-host:5432/opsflow"
JWT_SECRET="[generate-strong-random-key]"
JWT_REFRESH_SECRET="[generate-strong-random-key]"
SUPERADMIN_PASSWORD="[strong-password]"
ENCRYPTION_KEY="[32-char-key]"
PORT=5000
FRONTEND_URL="https://your-frontend-domain.com"
NODE_ENV="production"
```

---

## 🔒 Security Checklist

- ✅ JWT tokens with 7-day expiration
- ✅ Refresh token rotation on login
- ✅ AES-256 encryption for sensitive data
- ✅ Role-based access control on all routes
- ✅ Audit logging for compliance
- ✅ CORS enabled for frontend domain only
- ✅ Password hashing with bcryptjs
- ✅ Input validation on all endpoints

---

## 📚 API Documentation

All endpoints accept `Content-Type: application/json`.

### Login Example

```bash
curl -X POST http://localhost:5000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"muhammad.khalid@opsflow.com","password":"Admin@2024"}'
```

Response:
```json
{
  "success": true,
  "data": {
    "accessToken": "eyJhbGciOiJIUzI1NiIs...",
    "refreshToken": "eyJhbGciOiJIUzI1NiIs...",
    "user": {
      "id": "uuid",
      "email": "muhammad.khalid@opsflow.com",
      "name": "Muhammad Khalid",
      "role": "Super Admin"
    }
  }
}
```

---

## 🐛 Troubleshooting

### Database Connection Error

```
Error: connect ECONNREFUSED 127.0.0.1:5432
```

**Solution**: Ensure PostgreSQL is running and `DATABASE_URL` is correct.

### JWT Errors

```
"Invalid or expired token"
```

**Solution**: Check JWT_SECRET matches deployment config.

### Migration Issues

```bash
# Reset database (development only)
npx prisma migrate reset

# Force migration
npx prisma migrate resolve --rolled-back <migration-name>
```

---

## 📧 Support

For issues or feature requests, contact: **support@opsflow.com**

---

## 📄 License

MIT License © OpsFlow Technologies 2026
