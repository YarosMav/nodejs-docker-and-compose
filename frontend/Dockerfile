# Стейдж 1: Сборка (Builder)
FROM node:16-alpine AS builder

WORKDIR /app

# Копируем зависимости
COPY package*.json ./
RUN npm ci

# Копируем исходный код и собираем проект
COPY . .
RUN npm run build

# Стейдж 2: Раздача статики (Production)
FROM nginx:latest AS production

# Копируем собранные файлы из первого этапа в папку Nginx
# В React по умолчанию папка сборки называется 'build'
COPY --from=builder /app/build /usr/share/nginx/html

# Копируем наш кастомный конфиг Nginx
COPY ./nginx/conf.d/default.conf /etc/nginx/conf.d/default.conf

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]