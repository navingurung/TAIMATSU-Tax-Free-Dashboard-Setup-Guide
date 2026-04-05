# 🚀 TAIMATSU Tax Free Dashboard Setup Guide

## 📦 Tech Stack
- Next.js (Frontend)
- Docker (Development environment)
- TypeScript

---

## 🛠 Initial Setup

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

### 3. Create `.env` file

```
touch .env
```

Add:

```
NEXT_PUBLIC_API_BASE_URL=your_api_url_here
```

---

### 4. Create `Dockerfile.dev`

```
touch Dockerfile.dev
```

```
FROM node:20-alpine

WORKDIR /app

COPY package.json package-lock.json* ./
RUN npm install

COPY . .

EXPOSE 3001

CMD ["npm", "run", "dev"]
```

---

### 5. Edit(copy & paste) on `docker-compose.yml`


```
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

### 6. Update `package.json`

Change:

```
"dev": "next dev"
```

to:

```
"dev": "next dev -p 3001"
```

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

Even though Docker installs dependencies, VS Code runs locally and needs `node_modules`.

---

## 🐛 Troubleshooting

### `react/jsx-runtime` error

```
npm install
```

### Port issue

Ensure:

```
"dev": "next dev -p 3001"
```

---

## ✅ Final Setup Flow

```
git clone https://github.com/Fuga-taimatsu/taimatsu-tax-free-dashboard.git
cd taimatsu-tax-free-dashboard
npm install
docker compose up --build
```
