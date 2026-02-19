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

# ─── Environment ──────────────────────────────────────────────────────────────
ENV PORT=4000
ENV NODE_ENV=production
ENV USER_SERVICE_URL=http://user-service:4001
ENV NOTE_SERVICE_URL=http://note-service:4002

EXPOSE 4000

CMD ["node", "dist/server.js"]
