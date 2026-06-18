FROM node:20-alpine AS build

WORKDIR /app

COPY package*.json ./
RUN npm ci --only=production

COPY src/ ./src/

FROM node:20-alpine AS runtime

LABEL org.opencontainers.image.source="https://github.com/willdrz/devhub-campus-tp2"

WORKDIR /app

COPY --from=build /app/node_modules ./node_modules
COPY --from=build /app/src ./src

RUN addgroup -S appgroup && adduser -S appuser -G appgroup
USER appuser

ENV PORT=8080 \
    LOG_LEVEL=info \
    NODE_ENV=production

EXPOSE 8080

CMD ["node", "src/index.js"]
