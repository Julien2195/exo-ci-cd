# Guide CI/CD pour TodoList App

Ce document décrit la configuration CI/CD mise en place pour ce projet.

## Workflows GitHub Actions

### CI (Intégration Continue)

Le workflow CI (`ci.yml`) s'exécute à chaque push sur la branche `main` et pour chaque pull request vers `main`. Il effectue les opérations suivantes :

- Installation des dépendances
- Vérification du code avec ESLint
- Exécution des tests unitaires avec génération de rapports de couverture
- Publication des rapports de couverture sur Codecov

### CD (Déploiement Continu)

Le workflow CD (`cd.yml`) s'exécute après le succès du workflow CI et effectue :

- Construction de l'application
- Déploiement sur GitHub Pages

Des configurations alternatives pour Vercel et Netlify sont disponibles en commentaires dans le fichier.

### Docker

Le workflow Docker (`docker.yml`) construit et publie l'image Docker de l'application :

- Construction de l'image à chaque push sur `main`
- Publication de l'image sur GitHub Container Registry (ghcr.io)
- Versionnement automatique des images basé sur les tags Git

## Docker

### Utilisation locale

Pour exécuter l'application en local avec Docker :

```bash
# Construction de l'image
docker build -t todolist-app .

# Exécution du conteneur
docker run -p 8080:80 todolist-app
```

Ou avec Docker Compose :

```bash
docker-compose up
```

L'application sera accessible à l'adresse http://localhost:8080

## Configuration requise

Pour que la CI/CD fonctionne correctement, vous devez configurer les secrets suivants dans votre dépôt GitHub :

- `CODECOV_TOKEN`: Token d'accès pour Codecov (pour les rapports de couverture)

Pour les déploiements alternatifs :
- Pour Vercel : `VERCEL_TOKEN`, `VERCEL_ORG_ID`, `VERCEL_PROJECT_ID`
- Pour Netlify : `NETLIFY_AUTH_TOKEN`, `NETLIFY_SITE_ID`

## Bonnes pratiques

1. **Branches protégées** : Configurez la branche `main` comme protégée et exigez que les tests CI passent avant de fusionner les pull requests.

2. **Versionnement sémantique** : Utilisez des tags Git suivant le format `vX.Y.Z` pour déclencher des builds de versions spécifiques.

3. **Revue de code** : Exigez des revues de code avant de fusionner les pull requests.

4. **Tests** : Maintenez une bonne couverture de tests pour garantir la qualité du code. 
