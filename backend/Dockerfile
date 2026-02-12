# Стейдж 1: Сборка
FROM node:16-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# Стейдж 2: Финальный образ
FROM node:16-alpine AS production
WORKDIR /app
# Устанавливаем pm2 глобально по заданию
RUN npm install pm2 -g
# Копируем только зависимости для работы (без dev-зависимостей)
COPY --from=builder /app/package*.json ./
RUN npm ci --omit=dev && npm cache clean --force
# Копируем билд и конфиг pm2
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/ecosystem.config.js ./ 

EXPOSE 4000
# Запуск через pm2-runtime
CMD ["pm2-runtime", "start", "ecosystem.config.js"]