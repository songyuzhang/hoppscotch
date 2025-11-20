# -----------------------------
# 1. Builder Stage
# -----------------------------
FROM node:20-bullseye AS builder

WORKDIR /app

# Install pnpm
RUN npm install -g pnpm

# Copy workspace files
COPY pnpm-workspace.yaml ./
COPY pnpm-lock.yaml ./
COPY package.json ./

# Copy packages
COPY packages ./packages
COPY libs ./libs

# Install only necessary workspace dependencies
RUN pnpm install --frozen-lockfile

# Build backend (required for web)
WORKDIR /app/packages/hoppscotch-backend
RUN pnpm build

# Build selfhost web
WORKDIR /app/packages/hoppscotch-selfhost-web
RUN pnpm build


# -----------------------------
# 2. Final Stage
# -----------------------------
FROM caddy:2-alpine

WORKDIR /site

COPY packages/hoppscotch-selfhost-web/Caddyfile /etc/caddy/Caddyfile

# Copy web build output
COPY --from=builder /app/packages/hoppscotch-selfhost-web/dist/ /site/

EXPOSE 8080
