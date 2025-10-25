# -------------------------------
# Stage 1: Build frontend
# -------------------------------
FROM node:20 AS frontend
WORKDIR /app

# Copy frontend files
COPY package.json package-lock.json vite.config.js index.html ./
COPY src ./src

# Install dependencies and build React frontend
RUN npm install
RUN npm run build

# -------------------------------
# Stage 2: Build backend container
# -------------------------------
FROM python:3.13-slim
WORKDIR /app

# Install backend dependencies
COPY backend/requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy backend code
COPY backend/ ./

# Copy frontend build into backend static folder
COPY --from=frontend /app/dist ./backend/static

# Set environment variable for Cloud Run
ENV PORT=8080

# Start backend server (FastAPI example)
CMD ["uvicorn", "backend.main:app", "--host=0.0.0.0", "--port=8080"]