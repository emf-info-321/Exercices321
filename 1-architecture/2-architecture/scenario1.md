# 🟦 SCÉNARIO 1 — SnapWave 🚀  
### (Startup média – fort potentiel de croissance)

## Contexte

SnapWave est une startup fondée par 3 anciens étudiants en informatique.  
Leur idée : une application mobile où les utilisateurs publient des “moments” sous forme de photos et de courtes vidéos stylisées.

L’application permet :

- Upload de photos et vidéos
- Application de filtres et effets (style cinéma, rétro, IA simple)
- Génération automatique de :
  - thumbnails
  - versions compressées
  - formats optimisés mobile / web
- Timeline personnalisée
- Likes et commentaires
- Notifications en temps réel
- Statistiques d’engagement

## Contraintes

- Trafic imprévisible (un influenceur peut créer un pic massif)
- Traitement vidéo coûteux en CPU
- Les fonctionnalités évoluent rapidement
- Certaines parties peuvent être momentanément indisponibles sans bloquer tout le système (ex : stats ou modération)
- L’équipe veut héberger sur une infrastructure cloud moderne

## Travail demandé

Proposer une architecture adaptée :

- Comment découper l’application ?
- Comment gérer les pics de charge ?
- Quels composants doivent pouvoir scaler indépendamment ?
- Quels services pourraient être externalisés ?
- Comment assurer la résilience ?
