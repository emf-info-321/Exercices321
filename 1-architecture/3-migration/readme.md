# 🔥 Exercice – Choisir une stratégie de migration

## 🎯 Objectif

Être capable de :

- Comprendre la différence entre **Big Bang** et **Strangler Pattern**
- Choisir une stratégie de migration adaptée à un contexte donné
- Identifier les risques d’une transformation de système

---

## 🕒 Durée

15–20 minutes

---

# 🧩 Contexte

Une entreprise utilise depuis 8 ans une application monolithique pour :

- Gestion des clients
- Gestion des commandes
- Facturation
- Reporting

Problèmes actuels :

- Code difficile à maintenir
- Déploiement long (tout redéployer à chaque modification)
- Une seule base de données centrale
- Certaines parties doivent désormais supporter beaucoup plus de trafic (ex : commandes en ligne)
- L’équipe souhaite moderniser progressivement l’architecture

L’entreprise veut évoluer vers une architecture plus modulaire et scalable.

---

# 🟦 Travail individuel (5–7 minutes)

Répondez aux questions suivantes :

1. Quelle stratégie recommandez-vous ?
   - Big Bang - Migrer tout en une seule fois
   - Strangler Pattern - Migrer petit à petit (morceau par morceau de l'application)

2. Pourquoi ce choix ?

3. Quels sont les principaux risques de cette stratégie ?

4. Quels seraient les impacts sur :
   - Les utilisateurs ?
   - Les développeurs ?
   - L’exploitation ?

---

# 🟦 Discussion classe (10–15 minutes)

Chaque apprenti présente brièvement :

- Sa stratégie
- Son argument principal
