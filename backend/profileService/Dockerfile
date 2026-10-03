# ---------- Stage 1: Dependencies ----------
FROM node:24-alpine AS dependencies

WORKDIR /app

COPY package*.json ./

RUN npm ci --omit=dev


# ---------- Stage 2: Production ----------
FROM node:24-alpine AS production

WORKDIR /app

COPY --from=dependencies /app/node_modules ./node_modules

COPY index.js ./
COPY package*.json ./

EXPOSE 3002

CMD ["node", "index.js"]
