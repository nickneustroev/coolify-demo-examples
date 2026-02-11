# Build stage
FROM node:22.22 AS build
WORKDIR /app

# Включаем pnpm через corepack
RUN corepack enable && corepack prepare pnpm@latest --activate

# Копируем манифесты и конфиг pnpm ДО установки зависимостей
COPY package.json pnpm-lock.yaml pnpm-workspace.yaml ./

# Устанавливаем зависимости строго по lock-файлу
RUN pnpm install --frozen-lockfile

# Копируем остальной проект
COPY . .

# Сборка
RUN pnpm run build

# Production stage
FROM node:22.22
WORKDIR /app

# Копируем только результат билда
COPY --from=build /app/.output/ ./

ENV PORT=3000
ENV HOST=0.0.0.0
EXPOSE 3000

CMD ["node", "server/index.mjs"]
