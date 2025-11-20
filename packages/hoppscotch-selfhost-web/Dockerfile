# -----------------------------
# 1. Builder Stage (Node 20)
# -----------------------------
FROM node:20-bullseye AS builder

WORKDIR /app

RUN npm install -g pnpm

# Copy workspace files
COPY pnpm-workspace.yaml pnpm-lock.yaml package.json ./

# Copy only web + libs
COPY packages/hoppscotch-selfhost-web ./packages/hoppscotch-selfhost-web
COPY libs ./libs

# Install only web deps (skip backend)
RUN pnpm install --filter hoppscotch-selfhost-web... --frozen-lockfile

# Build the web
WORKDIR /app/packages/hoppscotch-selfhost-web
RUN pnpm build


# -----------------------------
# 2. Final Stage — Caddy
# -----------------------------
FROM caddy:2-alpine

WORKDIR /site

COPY packages/hoppscotch-selfhost-web/Caddyfile /etc/caddy/Caddyfile
COPY --from=builder /app/packages/hoppscotch-selfhost-web/dist/ /site/

EXPOSE 8080
