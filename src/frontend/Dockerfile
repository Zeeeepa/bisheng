FROM node:18-alpine as frontend_build

# 启用 Corepack（Node.js 内置的包管理器工具）
RUN corepack enable

# 安装特定版本的 pnpm（可选）
RUN corepack prepare pnpm@latest --activate

ARG BACKEND
WORKDIR /app
COPY . /app

# 使用 pnpm 安装依赖并构建
RUN cd /app/platform && \
    pnpm install && \
    pnpm build
RUN cd /app/client && npm install --force --registry=https://registry.npmmirror.com && npm run build

FROM nginx
COPY --from=frontend_build /app/platform/build/ /usr/share/nginx/html/platform
COPY --from=frontend_build /app/client/build/ /usr/share/nginx/html/client
COPY /nginx.conf /etc/nginx/conf.d/default.conf
