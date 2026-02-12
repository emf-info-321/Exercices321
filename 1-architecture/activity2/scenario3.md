# 🟩 SCÉNARIO 3 — TimeTrack Pro 🕒  
### (Application interne PME)

## Contexte

TimeTrack Pro est une application interne pour une PME de 60 employés.

Elle doit permettre :

- Saisie des heures de travail
- Validation par les managers
- Export mensuel (PDF / Excel)
- Gestion des rôles (employé, manager, admin)
- Authentification via le système interne de l’entreprise (SSO possible)

## Contraintes

- Charge stable et prévisible (heures de bureau)
- Peu d’évolution fonctionnelle prévue
- Budget limité
- Équipe IT réduite
- Sauvegardes régulières obligatoires
- Hébergement possible sur une machine virtuelle louée chez un fournisseur cloud

## Travail demandé

Proposer une architecture pragmatique :

- Faut-il découper en plusieurs services ?
- Où déployer l’application ?
- Comment gérer la base de données ?
- Comment assurer la sécurité et les sauvegardes ?
- Quelle solution cloud serait adaptée ?
