# Exercice : HTTP, Load Balancer, API Gateway et Traefik

**Durée** : 2–4 périodes  
**Format** : Individuel (ou binôme)  
**Matériel** : PC + Docker + Docker Compose + navigateur + curl  

## 🎯 Objectifs
À la fin de l’exercice, vous devez être capable de :
- Expliquer **HTTP** (requête/réponse, méthodes, headers, statuts)
- Expliquer le rôle d’un **Load Balancer (LB)**
- Expliquer le rôle d’un **API Gateway (APIGW)**
- Mettre en place **Traefik** comme reverse proxy / LB / gateway avec Docker

---

## Partie 1 — Comprendre HTTP (pratique)

### 1) Démarrer un serveur HTTP simple
Créez un dossier `http-lab` et ajoutez ce fichier `docker-compose.yml` :

```yaml
services:
  whoami:
    image: traefik/whoami:v1.10.1
    container_name: whoami
    ports:
      - "8081:80"
```

Démarrez :
```code
docker compose up -d
```

---

## 🧪 Étape 2 — Observer une requête HTTP

Dans le navigateur :

http://localhost:8081

Avec curl :

```code
curl -i http://localhost:8081
```

---

## ❓ Questions

1. Quelle est la méthode HTTP utilisée ?
2. Quel est le code de statut retourné ?
3. Quels headers sont présents ?
4. Quelle est la différence entre :
   - GET
   - POST
   - PUT
   - DELETE ?

---

## 🧪 Étape 3 — Tester d’autres méthodes

```
curl -i -X POST http://localhost:8081
```

```
curl -i -X GET http://localhost:8081
```

---

## ❓ Questions

1. Que signifie le code 200 ?
2. Que signifie 404 ?
3. Que signifie 500 ?
4. Que contient une requête HTTP ?

---

# Partie 2 — Comprendre un Load Balancer (LB)

## 🎯 Objectif

Comprendre pourquoi et comment on répartit la charge.

---

## 📌 Rappel théorique

Un **Load Balancer** permet de :

- Répartir les requêtes entre plusieurs instances
- Améliorer la disponibilité
- Permettre la scalabilité horizontale

---

## 🛠 Étape 1 — Simuler plusieurs instances

Lancer plusieurs instances du service :

```
docker compose up -d --scale whoami=3
```

Vérifier :

```
docker ps
```

---

## ❓ Questions

1. Pourquoi lancer plusieurs instances ?
2. Que se passe-t-il si une instance tombe ?
3. Avons-nous déjà un Load Balancer ici ? Pourquoi ?

---

# Partie 3 — Comprendre un API Gateway

## 🎯 Objectif

Comprendre la différence entre LB et API Gateway.

---

## 📌 Définition

Un **API Gateway** est un point d’entrée unique qui :

- Route vers différents services
- Peut gérer l’authentification
- Peut appliquer des règles (rate limiting)
- Centralise la configuration

---

## 🧠 Analyse

Pour chaque cas, indiquez :

- LB
- APIGW
- Les deux

1. Répartir le trafic entre 3 serveurs identiques  
2. Ajouter une authentification devant plusieurs services  
3. Exposer `/users` vers un service et `/orders` vers un autre  
4. Limiter à 50 requêtes/seconde  
5. Rediriger HTTP vers HTTPS  

Justifiez vos réponses.

---

# Partie 4 — Mettre en place Traefik

## 🎯 Objectif

Utiliser Traefik comme :

- Reverse proxy
- Load Balancer
- API Gateway

---

## 🛠 Étape 1 — Configuration

Remplacer votre `docker-compose.yml` par :

```yaml
services:
  traefik:
    image: traefik:v3.1
    command:
      - --api.dashboard=true
      - --api.insecure=true
      - --providers.docker=true
      - --providers.docker.exposedbydefault=false
      - --entrypoints.web.address=:80
    ports:
      - "80:80"
      - "8080:8080"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock:ro

  whoami:
    image: traefik/whoami:v1.10.1
    labels:
      - traefik.enable=true
      - traefik.http.routers.whoami.rule=PathPrefix(`/whoami`)
      - traefik.http.routers.whoami.entrypoints=web
      - traefik.http.services.whoami.loadbalancer.server.port=80

  echo:
    image: ealen/echo-server:latest
    labels:
      - traefik.enable=true
      - traefik.http.routers.echo.rule=PathPrefix(`/echo`)
      - traefik.http.routers.echo.entrypoints=web
      - traefik.http.services.echo.loadbalancer.server.port=80
```

Démarrer :

```
docker compose up -d
```

---

## 🧪 Étape 2 — Tester le routage

Dashboard :

http://localhost:8080

Tester :

http://localhost/whoami  
http://localhost/echo  

---

## ❓ Questions

1. Qui fait le routage ?
2. Pourquoi Traefik est un API Gateway ici ?
3. Où se trouve le Load Balancing ?

---

## 🛠 Étape 3 — Activer le Load Balancing

```
docker compose up -d --scale whoami=3
```

Tester plusieurs fois :

```
curl http://localhost/whoami
```

---

## ❓ Questions

1. Voyez-vous une différence entre les réponses ?
2. Comment Traefik répartit-il les requêtes ?
3. Pourquoi cela améliore-t-il la scalabilité ?

---

# 📦 Livrables

- Fichier docker-compose.yml
- Réponses aux questions
- Capture du dashboard Traefik
- Preuve du load balancing

---

# ✅ Critères de réussite

- HTTP est correctement expliqué
- LB et API Gateway sont distingués clairement
- Traefik fonctionne
- Le routage et le load balancing sont observables