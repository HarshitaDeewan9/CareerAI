# Stage 1: Build frontend
FROM node:20 AS frontend
WORKDIR /app

COPY package.json package-lock.json vite.config.js index.html ./
COPY src ./src

RUN npm install
RUN npm run build

# Stage 2: Build backend container
FROM python:3.13-slim
WORKDIR /app

COPY backend/requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY backend/ ./backend

# Copy frontend build
COPY --from=frontend /app/dist ./backend/static

ENV PORT 8080
EXPOSE 8080

# Start backend
CMD ["sh", "-c", "uvicorn backend.main:app --host 0.0.0.0 --port ${PORT}"]
