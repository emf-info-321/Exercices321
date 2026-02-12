# 🟨 SCÉNARIO 2 — RoboMesh 🤖  
### (Réseau de robots collaboratifs)

## Contexte

Lors d’un grand salon technologique, 15 robots autonomes se déplacent dans un hall.

Chaque robot :

- Possède un Raspberry Pi intégré
- Exécute du code localement
- Dispose de capteurs (distance, batterie, orientation)
- Peut communiquer avec les autres robots via Wi-Fi

Les robots doivent :

- Partager leur position
- Indiquer leur état (batterie faible, mission en cours)
- Se coordonner pour éviter de couvrir la même zone
- Continuer à fonctionner même si certains robots quittent le réseau
- Réagir rapidement aux événements locaux

## Contraintes

- Réseau instable et parfois saturé
- Certains robots peuvent tomber en panne
- Les décisions doivent parfois être prises localement
- Pas de garantie qu’une connexion externe soit disponible

## Travail demandé

Proposer une architecture de communication :

- Comment les robots échangent-ils les informations ?
- Faut-il un point central ?
- Comment gérer la perte d’un robot ?
- Comment éviter les incohérences ou doublons ?
- Où s’exécute la logique principale ?
