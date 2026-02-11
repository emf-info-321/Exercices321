# Module 321 – Programmer des systèmes distribués  
## Thème 1 : Keycloak  
### Exercice 1 : Installation et utilisation de Keycloak

---

## Informations générales

**Durée** : 4 périodes  
**Format** : Individuel  
**Matériel** : PC individuel  

---

# Partie 1 : Mise en place

## 🎯 Objectif

Mettre en place un serveur d’identité **Keycloak** et comprendre ses concepts de base :

- Realm  
- Users  
- Clients  
- Rôles  

Documentation officielle :  
https://www.keycloak.org/

---

## 📚 Contexte

Dans une architecture distribuée, l’authentification et l’autorisation sont externalisées dans un service d’identité.

**Keycloak** permet :

- de gérer les utilisateurs
- de gérer les rôles
- de fournir des tokens JWT aux applications

---

## 🛠 Travail demandé

1. Démarrer Keycloak avec **Docker** (via `docker-compose`)
2. Accéder à l’interface d’administration
3. Créer les éléments suivants :

   - Un **realm** nommé `exercice1`
   - Un **client public** nommé `exercice1-client`
   - Un **utilisateur** (exemple : `alice`)
   - Un **rôle de realm** nommé `student`

4. Assigner le rôle `student` à l’utilisateur
5. Vérifier que l’utilisateur peut se connecter via l’interface Keycloak

---

## 📦 Livrables

- Le fichier `docker-compose.yml`
- Une courte description des éléments créés :
  - realm
  - client
  - utilisateur
  - rôle
- Les URLs importantes :
  - **Issuer**
  - **JWKS URI**
  - **Token endpoint**

---

## ✅ Critères de réussite

- Keycloak démarre sans erreur
- L’interface d’administration est accessible
- L’utilisateur peut se connecter
- Le rôle est visible dans le compte utilisateur

---

# Partie 2 : Récupération d’un token JWT

## 🎯 Objectif

Obtenir un **token JWT** depuis Keycloak et analyser son contenu.

---

## 📚 Contexte

Avant d’intégrer une application frontend ou backend, il est essentiel de vérifier que l’émission des tokens fonctionne correctement.

Utilisez le Keycloak mis en place pour la classe à l’URL suivante :  http://keycloak1.emfnet.ch


---

## 🛠 Travail demandé

1. Effectuer une requête HTTP pour récupérer un **token d’accès** (Password Grant)
2. Copier le `access_token`
3. Décoder le token (https://jwt.io ou équivalent)
4. Identifier dans le token :

   - `iss`
   - `preferred_username`
   - les rôles (`realm_access.roles`)
   - la date d’expiration

---

## 📦 Livrables

- Un exemple de token valide
- Une capture d’écran ou description du contenu décodé

---

## ✅ Critères de réussite

- Le token est généré sans erreur
- Les informations utilisateur sont présentes
- L’issuer correspond au realm configuré

