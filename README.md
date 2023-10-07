# Entretien Yield Studio
Ce repo présente quelques projets que je juge intéressants pour avoir une idée de ce que je fais.

## Smartofus (Laravel/ReactJS)
Site: [Smartofus](https://www.smartofus.net/)

Le code source étant privé, je vais plutôt décrire quelques fonctionnements intéressants. Je suis seul à travailler sur ce projet actuellement.

Ce projet est un site communataire contenant divers outils pour aider les joueurs d'un jeu vidéo. Il s'agit d'un de mes plus gros projets: au demeurant simple, il a été complexe au niveau de l'architecture, de la gestion des données et du scripting notamment.

Le site a été codé avec l'approche "tests first", à savoir le TDD. Un outil de code coverage, ici sonarqube, a été mit en place, afin de m'assurer que les parties importantes sont testées.

Des linters, un pour le PHP, un pour le typescript, ont été mis en place, ainsi qu'un détecteur de "code smells". L'approche de clean code a été mise en avant.

### Obtention des données

Les données du jeu étant cryptées, il s'agissait d'abord de les obtenir d'une certaine manière. Après avoir essayé de parser l'encyclopédie présente sur le site, cette dernière étant boguée, j'ai décidé de décrypter les données fournies par le jeu en l'installant.

Pour ce faire, j'ai décompilé le jeu, et fouillé les scripts, afin de comprendre la logique de cryptage des données. J'ai ensuite créé mes propres scripts (node/typescript) pour décrypter les fichiers, et produire des JSON.

Ces JSON sont ensuite lus, et insérés en base de données (ceci est fait côté Laravel, je lance une commande qui va lire les JSON). Toutes les données venant du jeu, elles sont également traduites dans différentes langues. La base de données a été prévue pour être traduite dans plusieurs langues.

### Gestion des données
Le jeu ayant bientôt 20 ans, il y a une quantité considérable de données. Certaines tables de la base de données atteignent le million de lignes, et plus encore. J'ai donc mis en place plusieurs systèmes d'optimisation: la base évidemment, avec les indexs, les tailles de données adaptées à la donnée, et la mise en cache des requêtes gourmandes et fréquentes. Les requêtes sont également optimisées au possible.

J'ai mis en place Laravel Telescope sur le site pour visualiser les requêtes lourdes, les fonctionnalités plus utilisées, plus lentes, etc. J'adapte mes focus selon les rapports de Telescope.

### DevOps
A chaque merge, une batterie de tests est jouée pour s'assurer que rien n'a été brisé sur le site. Si ces tests passent, le merge peut être effectué sur la branche "master".

Une fois une version finie et prête à être déployée, la branche "master" est versée sur la branche "stable". Celle-ci subit également une batterie de tests, cette fois d'intégration. Si les tests d'intégration passent, je déploie la branche sur le serveur en cliquant sur un bouton. A l'arrivée sur le serveur, les différents caches sont nettoyés, les packages sont mis à jour (composer et npm), les divers scripts de migration sont lancés, et les données du jeu sont mises à jour.

### Améliorations
Le site reçoit actuellement quelques dizaines à quelques centaines de visites quotidiennes. Il est question de mettre en place une optimisation de chargement des images: si un "Lazy Loading" a été implémenté (en dev), il s'agit désormais de mettre en place un CDN qui distribuerait les images également, afin que la charge ne soit pas sur mon VPS.

Diverses fonctionnalités sont également en cours d'ajout.

## Git Report (ReactJS)
Site: [Git Report](https://git-report.com/)

GitHub: [Git Report](https://github.com/adrien-nf/git-report-web)

Projet permettant de gérer des rapports à partir de son historique de commits, j'ai plutôt géré la partie front-end, tandis que mon collègue a géré la partie back-end (en C#). Parfois nous avions des sessions de Peer Programming. Ce site a été donc été réalisé à 2.

Le design du site a été réalisé en collaboration sur Figma, où nous avons faits des mockups.

## Minecraft Server Status (PHP Vanilla)
Package: [Minecraft Server Status](https://packagist.org/packages/adrien-nf/minecraft-server-status)

GitHub: [Minecraft Server Status](https://github.com/adrien-nf/minecraft-server-status)

Système de communication avec les serveurs Minecraft, il permet d'obtenir des informations sur l'état actuel du serveur: joueurs en ligne, plugins, "Message of the Day", ... Ce projet a été réalisé seul.

J'ai développé cette librairie pour l'utiliser sur un site de création de serveurs Minecraft, où un joueur lambda peut créer un serveur entièrement par interface web, sans avoir besoin de s'y connaître. Ce site n'est pas actuellement en ligne.

### Améliorations
Le projet a été prévu pour la version Java des serveurs Minecraft. Je savais que je désirais l'étendre aux serveurs Bedrock, ainsi j'ai prévu la librairie pour aisément l'étendre au fonctionnement de ce second type de serveur, les protocoles étant différents.
