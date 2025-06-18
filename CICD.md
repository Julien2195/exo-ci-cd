# Guide CI/CD pour TodoList App

Ce document décrit la configuration CI/CD mise en place pour ce projet.

## Workflows GitHub Actions

### CI (Intégration Continue)

Le workflow CI (`ci.yml`) s'exécute à chaque push sur la branche `main` et pour chaque pull request vers `main`. Il effectue les opérations suivantes :

- Installation des dépendances
- Vérification du code avec ESLint
- Exécution des tests unitaires avec génération de rapports de couverture
- Publication des rapports de couverture sur Codecov (optionnel, si `CODECOV_TOKEN` est configuré)

### CD (Déploiement Continu)

Le workflow CD (`cd_netlify.yml`) s'exécute après le succès du workflow CI et effectue :

- Construction de l'application
- Déploiement sur Netlify

Pour que le déploiement fonctionne, vous devez configurer les secrets suivants dans les paramètres de votre dépôt GitHub :
- `NETLIFY_AUTH_TOKEN` : Votre token d'authentification Netlify
- `NETLIFY_SITE_ID` : L'ID de votre site Netlify

### Docker

Le workflow Docker (`docker.yml`) construit l'image Docker de l'application :

- Construction de l'image à chaque push sur `main`
- Le push vers un registre est désactivé par défaut (nécessite des tokens)

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

## Configuration des secrets Netlify

Pour configurer le déploiement sur Netlify, suivez ces étapes :

1. Créez un compte sur [Netlify](https://www.netlify.com/) si vous n'en avez pas déjà un.

2. Créez un nouveau site en important votre dépôt GitHub ou en glissant-déposant votre dossier `dist`.

3. Une fois le site créé, récupérez l'ID du site :
   - Allez dans les paramètres du site (Site settings)
   - L'ID du site se trouve dans la section "Site information"

4. Créez un token d'accès personnel :
   - Allez dans votre compte utilisateur (User settings)
   - Sélectionnez "Applications"
   - Créez un nouveau token d'accès personnel

5. Ajoutez ces secrets dans votre dépôt GitHub :
   - Allez dans les paramètres de votre dépôt GitHub
   - Sélectionnez "Secrets and variables" > "Actions"
   - Ajoutez deux nouveaux secrets :
     - `NETLIFY_AUTH_TOKEN` : Votre token d'accès personnel
     - `NETLIFY_SITE_ID` : L'ID de votre site

Une fois ces secrets configurés, le workflow CD déploiera automatiquement votre application sur Netlify à chaque push sur la branche `main`.

## Bonnes pratiques

1. **Branches protégées** : Configurez la branche `main` comme protégée et exigez que les tests CI passent avant de fusionner les pull requests.

2. **Versionnement sémantique** : Utilisez des tags Git suivant le format `vX.Y.Z` pour déclencher des builds de versions spécifiques.

3. **Revue de code** : Exigez des revues de code avant de fusionner les pull requests.

4. **Tests** : Maintenez une bonne couverture de tests pour garantir la qualité du code. 
