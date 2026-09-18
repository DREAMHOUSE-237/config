# config

Dépôt de configuration centralisée de la plateforme **DREAMHOUSE237**, servi par [`config-service`](https://github.com/DREAMHOUSE-237/config-service) (Spring Cloud Config Server).

## Contenu

Un fichier `.properties` par service, nommé selon la convention `<nom-application-spring>.properties` (ou `<nom-application>-default.properties`) :

| Fichier | Service |
|---|---|
| `authentification-default.properties` | auth-service |
| `users-default.properties` | user-service |
| `publication-service.properties` | publication-service |
| `payment-service.properties` | payment-service |
| `proxy-service.properties` | proxy-service (routes du gateway) |
| `service-enregistrement.properties` | registry-service |
| `application.properties` | configuration partagée par défaut |

## Fonctionnement

Chaque microservice interroge `config-service` au démarrage (ex. `GET /AUTHENTIFICATION/default`), qui lit ces fichiers depuis ce repo Git et les sert au format JSON. Les valeurs sensibles (mots de passe, tokens) ne sont **pas** codées en dur ici : elles référencent des variables d'environnement (`${VAR}`) injectées au moment du déploiement via les secrets GitHub Actions.

## Modifier une configuration

1. Éditer le fichier `.properties` correspondant au service concerné
2. Commit + push sur `main`
3. Le service concerné doit être redémarré (ou son cache de configuration rafraîchi) pour prendre en compte le changement — un simple push sur ce repo ne déclenche pas de redéploiement automatique des autres services.

## Déploiement

Ce repo n'a pas de pipeline CI/CD propre : il est lu dynamiquement par `config-service` à l'exécution.
