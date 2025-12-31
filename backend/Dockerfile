FROM node:18 AS builder
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
FROM node:18-alpine
WORKDIR /app
RUN addgroup -S backend && adduser -S backendUser -G backend
RUN chown backendUser:backend /app
COPY --from=builder /app /app
USER backendUser
EXPOSE 3500
CMD ["node", "server.js"]