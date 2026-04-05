# 🚀 TAIMATSU Tax Free Dashboard Setup Guide

## 📦 Tech Stack
- Next.js (Frontend)
- Docker (Development environment)
- TypeScript

---

## 🛠 Initial Setup

### 0. Open Docker Desktop

Make sure Docker Desktop is installed and running before starting.

---

### 1. Clone the repository

```
git clone https://github.com/Fuga-taimatsu/taimatsu-tax-free-dashboard.git
cd taimatsu-tax-free-dashboard
```

---

### 2. Install dependencies (for VS Code)

```
npm install
```

> Required for VS Code to resolve TypeScript & React (`react/jsx-runtime` error)

---

### 3. Set up `.env` file

Request the `.env` file from your team.

Place the received `.env` file in the root directory of the project.

Example:

```
NEXT_PUBLIC_API_BASE_URL=your_api_url_here
```

---

### 4. Check `Dockerfile.dev`

Compare your `Dockerfile.dev` with the configuration below.  
If it is different, update it to match:

```dockerfile
FROM node:20-alpine

WORKDIR /app

COPY package.json package-lock.json* ./
RUN npm install

COPY . .

EXPOSE 3001

CMD ["npm", "run", "dev", "--", "-p", "3001"]
```

---

### 5. Check `docker-compose.yml`

Compare your `docker-compose.yml` with the configuration below.  
If it is different, update it to match:

```yaml
services:
  dashboard:
    build:
      context: .
      dockerfile: Dockerfile.dev
    environment:
      - NODE_ENV=development
      - NEXT_PUBLIC_API_BASE_URL=${NEXT_PUBLIC_API_BASE_URL}
    container_name: taimatsu-tax-free-dashboard
    ports:
      - "3001:3001"
    volumes:
      - .:/app
      - /app/node_modules
    networks:
      - taimatsu-network

networks:
  taimatsu-network:
    driver: bridge
```

---

### 6. Check `package.json`

Ensure the `dev` script is:

```
"dev": "next dev"
```

> ⚠️ Do NOT change this to include a port. Port configuration is handled in Docker.

---

### 7. Start the app

```
docker compose up --build
```

Open:

http://localhost:3001

---

## 🔁 Daily Development

```
docker compose up
```

---

## 🧠 Notes

- Docker runs the application.
- Local `npm install` is required for VS Code (TypeScript, IntelliSense).
- Port configuration is handled by Docker, not `package.json`.

---

## 🐛 Troubleshooting

### `react/jsx-runtime` error

```
npm install
```

---

### Port issue

Ensure Docker is running and:

```
CMD ["npm", "run", "dev", "--", "-p", "3001"]
```

is correctly set in `Dockerfile.dev`.

---

## ✅ Final Setup Flow

```
git clone https://github.com/Fuga-taimatsu/taimatsu-tax-free-dashboard.git
cd taimatsu-tax-free-dashboard
npm install
docker compose up --build
```
