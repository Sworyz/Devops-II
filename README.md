# DevOps II
Bienvenue sur le projet de DevOps II utilisant une base de donnée postgreSQL, une API en Flask ainsi qu'un Front en Flask.
Pour les informations sur les sources, réferez vous à ce lien : https://git.ehanor.fr/ehanor/cseed

```
Le projet CSEED, une API REST programmé avec Python et Flask, fait partie d'un plus grand projet voulant permettre à beaucoup d'utilisations d'avoir une API simple mais sécurisé pour leurs jeux. Ce projet fait partie principalement de The Seed Project by Ehanor, une stack MMO ouverte basé sur Unreal Engine 4.
```

Pour le développement de ce projet, nous utilisons l'intégration continue sous Kubernetes, que vous pourrez retrouver dans les sources au dessus, ainsi que la construction automatique d'image Docker - via un Gitlab Runner et Kaniko - qui sont envoyés directement sur le Dockerhub. C'est d'ailleurs elles que nous utilisons dans le deployment du Kubernetes.

# Notre Infrastructure
Voici notre configuration Kubernetes :
Ubuntu 20.04 Server avec microk8s
- master 8 CPU, 30Go ram, 100Go HDD
- noeuds 4 CPU, 8Go ram, 100Go HDD

Cette infrastructure est hébergé sous un serveur Proxmox et se trouve derrière un OPNSense avec IDS et un reverse proxy sous nginx avec paramètre de sécurité renforcé, ainsi, nous n'utilisons pas d'Ingress pour répartir les requêtes.


# Informations à savoir
Pour que le deployment du kubernetes fonctionne, il faut labéliser le master node en tant que strg=ok, pour se faire : `kubectl label nodes <node-name> strg=ok`.
Cela nous permet de nous assurer que postgreSQL se lance bien là où se trouve le stockage, étant donné que la persistance du stockage est activée entre les redémarrages.

Les requêtes entre le Front et le Back se font en interne via http://cseed-api-service:5000 et les requêtes du Back à la BDD via http://cseed-postgres-service:5432, configuré via le .env
Vous pouvez changer les infos de la base de données avec de créer le Docker, ou les adapter directement dans le Kubernetes.

Le PullPolicy des conteneurs cseed est aggressif via le paramètre `Always`, pour faciliter les tests de développement.

# Liens
Pour retrouver le projet en déploiement, c'est ici : https://front.cseed.ehanor.fr/
Actuellement, vous pouvez :
- vous enregistrer (email valide, username entre 3 et 30 caractères, ainsi qu'un mot de passe entre 12 et 32 caractères contenant chiffre, majuscule miniscule et caractère spécial)
- vous connecter