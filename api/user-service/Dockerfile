# ─── Stage 1: Builder ────────────────────────────────────────────────────────
FROM node:20-alpine AS builder

WORKDIR /app

COPY package*.json ./
COPY tsconfig.json ./

RUN npm ci

COPY src/ ./src/
RUN npx tsc --project tsconfig.json --outDir dist --rootDir src --module commonjs --moduleResolution node

# ─── Stage 2: Production ─────────────────────────────────────────────────────
FROM node:20-alpine AS production

WORKDIR /app

COPY package*.json ./
RUN npm ci --omit=dev

COPY --from=builder /app/dist ./dist

# ─── PostgreSQL connection (overridden by docker-compose) ─────────────────────
ENV PORT=4001
ENV NODE_ENV=production
ENV DATABASE_URL=postgresql://user_admin:user_secret@user-db:5432/user_db

EXPOSE 4001

CMD ["node", "dist/server.js"]
