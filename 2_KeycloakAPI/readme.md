# Module 321 – Programmer des systèmes distribués  
## Thème 1 : Keycloak  
### Exercice 2 : Utilisation de Keycloak

---

## Informations générales

**Durée** : 2 périodes  
**Format** : Individuel  
**Matériel** : PC individuel  

---

# Partie 1 : API protégée par Keycloak

## 🎯 Objectif

Créer une API **Node.js** capable de vérifier un **token JWT** émis par Keycloak.

---

## 📚 Contexte

Dans une architecture microservices, les services doivent vérifier les tokens JWT **sans contacter Keycloak à chaque requête**.

Cela est possible grâce au mécanisme **JWKS (JSON Web Key Set)**.

👉 Utilisez le Keycloak mis en place pour votre classe.

---

## 🛠 Travail demandé

1. Créer une API **Node.js** avec **Express**
2. Ajouter les routes suivantes :

   - `GET /health` → route publique
   - `GET /me` → route protégée par JWT
   - `GET /admin` → route protégée + rôle requis

3. Configurer la vérification JWT via le **JWKS de Keycloak**
4. Tester les routes :
   - sans token
   - avec token valide
   - avec token sans rôle requis

---

## 📦 Livrables

- Code source de l’API
- `Dockerfile`
- Description des tests effectués

---

## ✅ Critères de réussite

- `/health` est accessible sans token
- `/me` retourne les informations utilisateur avec un token valide
- `/admin` refuse l’accès sans le rôle requis

---

# Partie 2 : SPA – Login avec Keycloak

## 🎯 Objectif

Créer une **SPA (Single Page Application)** capable de s’authentifier via Keycloak.

---

## 📚 Contexte

Les applications frontend délèguent l’authentification à Keycloak via **OpenID Connect (OIDC)**.

---

## 🛠 Travail demandé

1. Créer une page **SPA (HTML + JavaScript)**
2. Ajouter :

   - Un bouton **Login**
   - Un bouton **Logout**
   - L’affichage des informations utilisateur

3. Utiliser le Keycloak de la classe pour l’authentification
4. Stocker temporairement le token côté client

---

## 📦 Livrables

- Fichiers de la SPA
- Description du flow de connexion (login → redirection → retour)

---

## ✅ Critères de réussite

- Redirection vers Keycloak lors du login
- Retour automatique vers la SPA après authentification
- Les informations utilisateur sont affichées

---

# Partie 3 : SPA – API protégée

## 🎯 Objectif

Faire communiquer la SPA avec une API protégée par JWT.

---

## 📚 Contexte

Une SPA consomme des APIs sécurisées en envoyant le token JWT dans les requêtes HTTP via le header :
'''
Authorization: Bearer <token>
'''

---

## 🛠 Travail demandé

1. Depuis la SPA, appeler la route protégée `/me`
2. Ajouter le header `Authorization` avec le token JWT
3. Afficher la réponse dans la page

---

## 📦 Livrables

- Code de la SPA modifiée
- Preuve d’un appel réussi (capture ou description)

---

## ✅ Critères de réussite

- `401 Unauthorized` sans token
- `200 OK` avec token valide
- Les données retournées sont affichées dans la page

