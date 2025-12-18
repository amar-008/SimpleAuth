<div align="center">

# SimpleAuth

### Modern Full-Stack Authentication System

A secure, production-ready authentication system demonstrating industry-standard practices with JWT, TypeScript, and modern web technologies.

[![Live Demo](https://img.shields.io/badge/Live_Demo-auth67.vercel.app-0A66C2?style=for-the-badge&logo=vercel&logoColor=white)](https://auth67.vercel.app)

**React** · **TypeScript** · **Express** · **PostgreSQL** · **TailwindCSS**

</div>

## The Problem

Building authentication from scratch is complex and error-prone. Many developers struggle with:

- Implementing secure password hashing and storage
- Managing JWT tokens and session handling correctly
- Setting up protected routes on both frontend and backend
- Preventing common security vulnerabilities (XSS, CSRF, SQL injection)

**SimpleAuth solves this** by providing a clean, well-documented reference implementation that demonstrates secure authentication patterns you can learn from or use as a starting point.

## Features

| Feature | Description |
|---------|-------------|
| **JWT Authentication** | Secure token-based auth with httpOnly cookies (3-hour expiry) |
| **Password Security** | Bcrypt hashing with 10 salt rounds for secure storage |
| **Input Validation** | Server-side validation using Zod schemas on all requests |
| **Protected Routes** | Middleware-based route protection on frontend and backend |
| **Type Safety** | Full TypeScript implementation across the entire stack |
| **Modern UI** | Clean, responsive design with VS Code-inspired dark theme |

### How It Works

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│  React Frontend │────▶│  Express API    │────▶│   PostgreSQL    │
│  (Vercel)       │◀────│  (Render)       │◀────│   Database      │
└─────────────────┘     └─────────────────┘     └─────────────────┘
        │                       │
        └── JWT in httpOnly ────┘
            Cookie
```

## Tech Stack

<table>
<tr>
<td align="center" width="50%">

### Frontend

![React](https://img.shields.io/badge/React_19-20232A?style=flat-square&logo=react&logoColor=61DAFB)
![TypeScript](https://img.shields.io/badge/TypeScript-007ACC?style=flat-square&logo=typescript&logoColor=white)
![Tailwind](https://img.shields.io/badge/Tailwind_CSS-38B2AC?style=flat-square&logo=tailwind-css&logoColor=white)
![Vite](https://img.shields.io/badge/Vite-646CFF?style=flat-square&logo=vite&logoColor=white)

- **React 19** with hooks and modern patterns
- **TanStack Query** for data fetching
- **React Router 7** for client-side routing
- **Axios** for HTTP requests

</td>
<td align="center" width="50%">

### Backend

![Node.js](https://img.shields.io/badge/Node.js-43853D?style=flat-square&logo=node.js&logoColor=white)
![Express](https://img.shields.io/badge/Express_5-000000?style=flat-square&logo=express&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=flat-square&logo=postgresql&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)

- **Express 5** REST API framework
- **JWT** for token-based authentication
- **Bcrypt** for password hashing
- **Zod** for schema validation

</td>
</tr>
</table>

## Getting Started

### Prerequisites

- Node.js 18+
- Docker and Docker Compose
- Git

### Installation

**1. Clone the repository**

```bash
git clone https://github.com/amarendra-008/SimpleAuth.git
cd SimpleAuth
```

**2. Start PostgreSQL database**

```bash
docker-compose up -d
```

**3. Set up the backend**

```bash
cd api
npm install
```

Create a `.env` file in the `api` directory:

```env
PORT=3000
DATABASE_URL=postgresql://postgres:password@localhost:5432/authdb
JWT_SECRET=your-secret-key-here
NODE_ENV=development
```

Start the backend server:

```bash
npm run dev
```

**4. Set up the frontend**

```bash
cd ../web
npm install
npm run dev
```

**5. Access the application**

- Frontend: http://localhost:5173
- Backend API: http://localhost:3000

## Architecture

### System Overview

```
┌──────────────────────────────────────────────────────────────────┐
│                         Frontend (React)                          │
├──────────────────────────────────────────────────────────────────┤
│  Components    │    Pages      │    Hooks     │    API Layer     │
│  - Navbar      │  - Home       │  - useAuth   │  - auth.ts       │
│  - Footer      │  - Login      │              │  - axios.ts      │
│  - Protected   │  - Signup     │              │                  │
│    Route       │  - Dashboard  │              │                  │
└───────────────────────────┬──────────────────────────────────────┘
                            │ HTTP (JWT Cookie)
┌───────────────────────────▼──────────────────────────────────────┐
│                         Backend (Express)                         │
├──────────────────────────────────────────────────────────────────┤
│  Routes        │  Controllers  │  Middleware  │    Utils         │
│  - authRoutes  │  - authCtrl   │  - auth      │  - jwt.ts        │
│                │               │  - validation│  - password.ts   │
└───────────────────────────┬──────────────────────────────────────┘
                            │ SQL
┌───────────────────────────▼──────────────────────────────────────┐
│                      PostgreSQL Database                          │
│                         (Docker)                                  │
└──────────────────────────────────────────────────────────────────┘
```

### Project Structure

```
SimpleAuth/
├── api/                      # Backend application
│   ├── src/
│   │   ├── controllers/      # Request handlers
│   │   ├── db/               # Database connection
│   │   ├── middleware/       # Auth & validation middleware
│   │   ├── routes/           # API routes
│   │   ├── types/            # TypeScript types
│   │   └── utils/            # Helper functions (JWT, bcrypt)
│   └── package.json
├── web/                      # Frontend application
│   ├── components/           # Reusable components
│   ├── pages/                # Page components
│   ├── src/
│   │   ├── api/              # API client functions
│   │   ├── hooks/            # Custom React hooks
│   │   └── types/            # TypeScript types
│   ├── styles/               # Style configurations
│   └── package.json
└── docker-compose.yml        # Docker configuration
```

## API Reference

### Register User

```http
POST /api/auth/signup
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `name` | string | Yes | User's full name |
| `username` | string | Yes | Unique username |
| `email` | string | Yes | Unique email address |
| `password` | string | Yes | User password |

**Response**

```json
{
  "user": {
    "id": 1,
    "name": "John Doe",
    "username": "johndoe",
    "email": "john@example.com"
  }
}
```

### Login

```http
POST /api/auth/login
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `email` | string | Yes | User email |
| `password` | string | Yes | User password |

### Logout

```http
POST /api/auth/logout
```

Clears the authentication cookie.

### Get Current User

```http
GET /api/auth/me
```

Requires authentication. Returns the currently logged-in user's details.

## Database Schema

### users

| Column | Type | Description |
|--------|------|-------------|
| `id` | SERIAL | Primary key |
| `name` | VARCHAR(255) | User's full name |
| `username` | VARCHAR(255) | Unique username |
| `email` | VARCHAR(255) | Unique email address |
| `password` | VARCHAR(255) | Bcrypt hashed password |
| `created_at` | TIMESTAMP | Account creation time |

## Deployment

### Vercel (Frontend)

The frontend is configured for Vercel deployment with `vercel.json`:

```json
{
  "rewrites": [{ "source": "/(.*)", "destination": "/" }]
}
```

### Render (Backend)

The backend includes a `render-build.sh` script for Render deployment:

```bash
npm install && npm run build
```

Set these environment variables on Render:

```
PORT=3000
NODE_ENV=production
DATABASE_URL=<your-render-postgresql-url>
JWT_SECRET=<generated-secret>
FRONTEND_URL=<your-vercel-url>
```

## Security Features

- Password hashing using bcrypt with 10 salt rounds
- JWT tokens stored in httpOnly cookies (3-hour expiry)
- Protected API endpoints with authentication middleware
- Input validation on all requests using Zod schemas
- CORS configuration for cross-origin requests
- SQL injection prevention through parameterized queries

## Roadmap

- [ ] **Email Verification** - Verify user email addresses
- [ ] **Password Reset** - Forgot password functionality
- [ ] **OAuth Integration** - Google/GitHub login
- [ ] **Rate Limiting** - Prevent brute force attacks
- [ ] **Refresh Tokens** - Implement token refresh mechanism
- [ ] **Two-Factor Auth** - Add 2FA support

## Contributing

Contributions are welcome! Here's how you can help:

1. **Fork** the repository
2. **Create** a feature branch (`git checkout -b feature/amazing-feature`)
3. **Commit** your changes (`git commit -m 'Add amazing feature'`)
4. **Push** to the branch (`git push origin feature/amazing-feature`)
5. **Open** a Pull Request

## License

This project is licensed under the MIT License.

---

<div align="center">

**Built by [@amarendra-008](https://github.com/amarendra-008)**

</div>