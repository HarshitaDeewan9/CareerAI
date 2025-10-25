# Stage 1: Build frontend
FROM node:20 AS frontend
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm install
COPY vite.config.js src ./ 
RUN npm run build

# Stage 2: Build backend container
FROM python:3.13-slim
WORKDIR /app
COPY backend/requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY backend/ ./
COPY --from=frontend /app/dist ./backend/static
ENV PORT=8080
CMD ["uvicorn", "backend.main:app", "--host=0.0.0.0", "--port=8080"]