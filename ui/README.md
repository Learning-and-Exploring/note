# 📝 Notion UI

[![React](https://img.shields.io/badge/React-18-61DAFB?style=for-the-badge&logo=react&logoColor=black)](https://reactjs.org/)
[![TypeScript](https://img.shields.io/badge/TypeScript-007ACC?style=for-the-badge&logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![Vite](https://img.shields.io/badge/Vite-646CFF?style=for-the-badge&logo=vite&logoColor=white)](https://vitejs.dev/)
[![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://www.docker.com/)

The frontend client for the Notion API microservices project, built with **React 18 + TypeScript + Vite**.

---

## 🏗 Tech Stack

| Tool                | Purpose                                  |
| :------------------ | :--------------------------------------- |
| **React 18**        | UI library                               |
| **TypeScript**      | Type safety                              |
| **Vite**            | Build tool & dev server                  |
| **React Router v6** | Client-side routing                      |
| **Axios**           | HTTP client for API calls                |
| **Nginx**           | Serves the production build (via Docker) |

---

## 📂 Project Structure

```bash
ui/
├── public/               # Static assets
├── src/
│   ├── assets/           # Images, fonts, icons
│   ├── components/       # Reusable UI components
│   ├── pages/            # Route-level page components
│   ├── services/         # Axios API call wrappers
│   ├── hooks/            # Custom React hooks
│   ├── types/            # Shared TypeScript types
│   ├── App.tsx           # Root component with router
│   └── main.tsx          # Entry point
├── Dockerfile            # Production Docker image
├── index.html
├── vite.config.ts
└── tsconfig.json
```

---

## 🚀 Getting Started

### Prerequisites

- Node.js **v18+**
- npm or yarn

### Install dependencies

```bash
npm install
```

### Run development server

```bash
npm run dev
```

App runs at **http://localhost:5173** by default.

---

## 🔌 API Integration

All API requests are proxied through **Nginx** (in Docker) or the Vite dev proxy to the **API Gateway** at port `4000`.

In development, add this to `vite.config.ts`:

```ts
server: {
  proxy: {
    '/api': 'http://localhost:4000',
  },
},
```

In production (Docker), Nginx handles the `/api/*` → `api-gateway:4000` routing automatically.

---

## 🐳 Docker

The UI is served as a static build behind Nginx. The `nginx/nginx.conf` at the project root handles routing.

```bash
# Build and run the full stack (from project root)
cd ../api
docker compose up --build
```

The UI is accessible at **http://localhost:80**.

---

## 📜 Available Scripts

| Command           | Description                      |
| :---------------- | :------------------------------- |
| `npm run dev`     | Start Vite dev server            |
| `npm run build`   | Build for production (`dist/`)   |
| `npm run preview` | Preview production build locally |
| `npm run lint`    | Run ESLint                       |
