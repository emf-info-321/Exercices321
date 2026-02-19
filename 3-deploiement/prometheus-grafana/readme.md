# 🧪 Exercice – Observabilité avec Node.js, Prometheus et Grafana

## 🎯 Objectif

À la fin de cet exercice, vous devez avoir :

- Un service Node.js exposant :
  - GET /health
  - GET /metrics
- Prometheus qui scrape les métriques du service
- Grafana affichant un dashboard avec au moins 2 panels

---

# 🧱 Partie 1 – Créer le service Node.js

## 1️⃣ Initialiser le projet

```bash
mkdir exo-observability-node
cd exo-observability-node
mkdir app prometheus
cd app
npm init -y
npm i express prom-client
```

---

## 2️⃣ Configurer le projet

Dans package.json, ajouter :

```json
"type": "module",
"scripts": {
  "start": "node index.js"
}
```

---

## 3️⃣ Créer le fichier index.js

Créer app/index.js :

```js
import express from "express";
import client from "prom-client";

const app = express();
const port = process.env.PORT || 3000;

// Collecte des métriques système (CPU, mémoire, event loop, etc.)
client.collectDefaultMetrics();

// Compteur personnalisé
const httpRequestsTotal = new client.Counter({
  name: "demo_http_requests_total",
  help: "Total number of HTTP requests",
  labelNames: ["method", "route", "status"],
});

// Middleware pour compter les requêtes
app.use((req, res, next) => {
  res.on("finish", () => {
    const route = req.route?.path || req.path;
    httpRequestsTotal.inc({
      method: req.method,
      route,
      status: String(res.statusCode),
    });
  });
  next();
});

// Route health
app.get("/health", (req, res) => {
  res.json({ status: "ok" });
});

// Route metrics
app.get("/metrics", async (req, res) => {
  res.set("Content-Type", client.register.contentType);
  res.end(await client.register.metrics());
});

app.listen(port, () => {
  console.log(`App listening on port ${port}`);
});
```

---

## 4️⃣ Tester localement (optionnel)

```bash
npm start
```

Puis :

```bash
curl http://localhost:3000/health
curl http://localhost:3000/metrics
```

Arrêter ensuite avec CTRL+C.

---

# 🐳 Partie 2 – Dockeriser l'application

Créer app/Dockerfile :

```dockerfile
FROM node:20-alpine

WORKDIR /app

COPY package*.json ./
RUN npm ci --omit=dev

COPY . .

ENV PORT=3000
EXPOSE 3000

CMD ["npm", "start"]
```

---

# 📊 Partie 3 – Configurer Prometheus

Créer prometheus/prometheus.yml :

```yaml
global:
  scrape_interval: 5s

scrape_configs:
  - job_name: "node-app"
    metrics_path: /metrics
    static_configs:
      - targets: ["app:3000"]
```

---

# 🐳 Partie 4 – Docker Compose

Créer docker-compose.yml à la racine :

```yaml
services:
  app:
    build: ./app
    ports:
      - "3001:3000"

  prometheus:
    image: prom/prometheus
    ports:
      - "9090:9090"
    volumes:
      - ./prometheus/prometheus.yml:/etc/prometheus/prometheus.yml:ro
    depends_on:
      - app

  grafana:
    image: grafana/grafana
    ports:
      - "3000:3000"
    depends_on:
      - prometheus
```

---

# 🚀 Partie 5 – Lancer la stack

Depuis la racine du projet :

```bash
docker compose up -d --build
```

---

# ✅ Partie 6 – Vérifications

## 🔎 Vérifier le service

```bash
curl http://localhost:3001/health
curl http://localhost:3001/metrics
```

---

## 📡 Vérifier Prometheus

Aller sur :

http://localhost:9090

Puis :

- Status → Targets
- Vérifier que node-app est UP

---

# 📈 Partie 7 – Configurer Grafana

Aller sur :

http://localhost:3000

Login par défaut :

admin / admin

---

## Ajouter Prometheus comme Data Source

- Connections → Data sources → Add data source
- Choisir Prometheus
- URL :

http://prometheus:9090

- Save & Test

---

# 📊 Partie 8 – Créer un Dashboard

Créer un dashboard avec au moins 2 panels.

---

## 🟢 Panel 1 – Target UP

```promql
up{job="node-app"}
```

---

## 🔵 Panel 2 – Requêtes HTTP par seconde

```promql
rate(demo_http_requests_total[1m])
```

---

## 🟣 Bonus – Mémoire utilisée

```promql
process_resident_memory_bytes
```

---

# 🔁 Générer du trafic

Dans un terminal :

```bash
for i in {1..50}; do curl -s http://localhost:3001/health > /dev/null; done
```

Observer les graphes dans Grafana.

---

# 🏁 Critères de réussite

Vous devez pouvoir démontrer :

- /health fonctionne
- /metrics expose des métriques
- Prometheus voit la target UP
- Grafana affiche au moins 2 panels avec des données
- Le compteur de requêtes augmente quand vous faites du trafic

---

# ❓ Questions de compréhension

1. Pourquoi Grafana utilise http://prometheus:9090 et pas localhost ?
2. Quelle est la différence entre /health et /metrics ?
3. Que signifie la métrique up ?
4. Pourquoi utilise-t-on rate() sur un compteur ?
