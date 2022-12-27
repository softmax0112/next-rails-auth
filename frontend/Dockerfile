# Install dependencies only when needed
FROM node:14.16.1-alpine AS deps
# RUN apk add --no-cache libc6-compat nasm autoconf automake bash libltdl libtool gcc make g++ zlib-dev
WORKDIR /frontend
# Copy from SRC->dockerfile-src (context/directory of Dockerfile) 
# DEST->absolute path or $WORKDIR/
COPY package.json yarn.lock ./
# RUN yarn install --network-timeout 1000000
RUN yarn
# ==================================================================

# Rebuild the source code only when needed
FROM node:14.16.1-alpine AS builder
WORKDIR /frontend
COPY --from=deps ./frontend/node_modules/ ./node_modules
COPY . .
# Next.js collects completely anonymous telemetry data about general usage.
# Learn more here: https://nextjs.org/telemetry
# Uncomment the following line in case you want to disable telemetry during the build.
# ENV NEXT_TELEMETRY_DISABLED 1
# RUN yarn build
# ==================================================================

# Production image, copy all the files and run next
# Used when running next build

FROM node:14.16.1-alpine AS runner
# Uncomment the following line in case you want to disable telemetry during runtime.
# ENV NEXT_TELEMETRY_DISABLED 1
# RUN apk add --no-cache libc6-compat nasm autoconf automake bash
# RUN addgroup --system --gid 1001 nodejs
# RUN adduser --system --uid 1001 nextjs

WORKDIR /frontend
# You only need to copy next.config.js if you are NOT using the default configuration
# COPY --from=builder next.config.js ./
# COPY --from=builder ./frontend/.next/ ./.next
COPY --from=builder ./frontend/package.json ./
COPY --from=builder ./frontend/public/ ./public/
COPY --from=builder ./frontend/node_modules/ ./node_modules

# Automatically leverage output traces to reduce image size 
# https://nextjs.org/docs/advanced-features/output-file-tracing
# COPY --from=builder --chown=nextjs:nodejs /app/.next/standalone ./
# COPY --from=builder --chown=nextjs:nodejs /app/.next/static ./.next/static
# USER nextjs
EXPOSE 3000
ENV PORT 3000

# CMD ["node", "server.js"]
CMD ["yarn", "dev"]
