FROM node:23-alpine AS base
WORKDIR /usr/src/app

FROM base AS deps
RUN apk add --no-cache libc6-compat
COPY package.json package-lock.json* ./
RUN npm install --omit=dev

FROM base AS builder
COPY --from=deps /usr/src/app /usr/src/app
COPY . .
RUN npm run build

FROM base AS runner
ENV NODE_ENV production
ENV PORT $APP_PORT
COPY --from=builder /usr/src/app/dist ./dist
COPY --from=deps /usr/src/app/node_modules ./node_modules
EXPOSE $APP_PORT
CMD ["node", "dist/main.js"]