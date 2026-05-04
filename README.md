# Auth0 + Express

Full-stack authentication app using Auth0 and Express on the backend, and React + Vite on the frontend.

## How it works

The backend manages the entire auth flow using `express-openid-connect`. The frontend never handles login directly — it just redirects to `http://localhost:3000/login` and the backend takes care of the rest.

After login, Auth0 redirects back to the app and the backend stores the session. The frontend reads user data by calling the backend API.

## Project structure

```
auth0withExpress/
├── backend/
│   ├── middleware/
│   │   └── auth.js        # Auth0 config via express-openid-connect
│   ├── index.js           # Express server + routes
│   ├── index.test.js      # Integration tests (Vitest)
│   └── .env.example       # Environment variable template
└── client/
    ├── src/
    │   ├── components/
    │   │   └── Profile/
    │   │       ├── Profile.jsx       # Profile page (Task A + B)
    │   │       └── Profile.test.jsx  # Unit tests (Vitest)
    │   ├── App.jsx
    │   └── main.jsx
    └── index.html
```

## Getting started

### 1. Clone the repo

```bash
git clone https://github.com/Robbhedonic/auth0withExpress.git
cd auth0withExpress
```

### 2. Install dependencies

```bash
cd backend && npm install
cd ../client && npm install
```

### 3. Configure environment variables

```bash
cp backend/.env.example backend/.env
```

Fill in your Auth0 credentials in `backend/.env`:

```
SECRET=your_random_secret
BASE_URL=http://localhost:3000
CLIENT_ID=your_auth0_client_id
ISSUER=https://your-tenant.us.auth0.com
CLIENT_URL=http://localhost:5173
PORT=3000
```

You can find `CLIENT_ID` and `ISSUER` in your [Auth0 Dashboard](https://manage.auth0.com) under Applications → your app.

> Make sure your Auth0 app has `http://localhost:3000/callback` as an **Allowed Callback URL** and `http://localhost:3000` as an **Allowed Logout URL**.

### 4. Run the app

```bash
# Terminal 1 — backend
cd backend && npm run dev

# Terminal 2 — frontend
cd client && npm run dev
```

- Backend: http://localhost:3000
- Frontend: http://localhost:5173

## API routes

| Method | Route | Auth required | Description |
|--------|-------|---------------|-------------|
| GET | `/` | No | Health check |
| GET | `/me` | No | Returns current user or null |
| GET | `/login` | No | Redirects to Auth0 login |
| GET | `/logout` | No | Logs out and redirects |
| GET | `/profile` | Yes | Returns logged-in user data |
| GET | `/secure-data` | Yes | Returns protected message + user |

## Running tests

```bash
# Frontend unit tests
cd client && npm test

# Backend integration tests
cd backend && npm test
```
