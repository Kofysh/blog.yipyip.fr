## Introduction au Reverse Proxy

Un reverse proxy, ou proxy inverse, agit comme un intermédiaire entre les clients et les serveurs, permettant d'accéder aux ressources web sur des serveurs internes tout en masquant leur identité. Contrairement aux proxies traditionnels qui protègent l'identité des clients, un reverse proxy sécurise les serveurs en centralisant le trafic et en appliquant des mesures de sécurité avancées.

### Avantages du Reverse Proxy

Les principaux avantages d'utiliser un reverse proxy incluent :

- **Sécurité renforcée :** En exposant uniquement le reverse proxy au trafic extérieur, la surface d'attaque est réduite pour les serveurs internes, permettant un contrôle plus fin des accès et une protection contre les attaques.
  
- **Gestion centralisée du trafic :** Tous les services peuvent être gérés via un seul point d'entrée, facilitant la configuration des politiques de sécurité, la répartition de charge et la gestion des connexions.
  
- **Optimisation des performances :** Le reverse proxy peut mettre en cache les contenus statiques, compresser les données et gérer efficacement la répartition de charge, améliorant ainsi les performances globales des applications web.

### Déploiement avec NGINX Proxy Manager

NGINX Proxy Manager est une solution populaire pour déployer et gérer un reverse proxy de manière conviviale. Voici comment le mettre en œuvre dans votre propre environnement d'auto-hébergement :

#### Configuration avec Docker Compose

Utilisez le fichier `docker-compose.yml` suivant pour déployer NGINX Proxy Manager :

```yaml
version: '3'
services:
  app:
    image: 'jc21/nginx-proxy-manager:latest'
    restart: unless-stopped
    ports:
      - '80:80'
      - '443:443'
      - '81:81'
    environment:
      DB_MYSQL_HOST: "db"
      DB_MYSQL_PORT: 3306
      DB_MYSQL_USER: "npm"
      DB_MYSQL_PASSWORD: "npm"
      DB_MYSQL_NAME: "npm"
    volumes:
      - ./npm_data:/data
      - ./npm_letsencrypt:/etc/letsencrypt
    networks:
      - npm_proxy

  db:
    image: 'jc21/mariadb-aria:latest'
    restart: unless-stopped
    environment:
      MYSQL_ROOT_PASSWORD: 'npm'
      MYSQL_DATABASE: 'npm'
      MYSQL_USER: 'npm'
      MYSQL_PASSWORD: 'npm'
    volumes:
      - ./npm_db:/var/lib/mysql
    networks:
      - npm_proxy

networks:
  npm_proxy:
    driver: bridge
```

#### Accès à l'Interface de Gestion

Après le déploiement, accédez à l'interface web de NGINX Proxy Manager via votre navigateur :

```plaintext
http://ip:81
```

Connectez-vous avec les identifiants par défaut :

- **Email :** `admin@example.com`
- **Mot de passe :** `changeme`

Assurez-vous de changer le mot de passe par défaut après la première connexion.

### Utilisation de NGINX Proxy Manager

Une fois connecté à l'interface, vous pouvez :

- Configurer des hôtes proxy pour exposer vos services web via des noms de domaine personnalisés.
- Gérer les redirections et les règles de routage pour optimiser la disponibilité des applications.
- Gérer les certificats SSL/TLS pour sécuriser les communications HTTPS avec vos services.

### Conclusion

En déployant NGINX Proxy Manager dans votre HomeLab, vous simplifiez la gestion de l'accès à vos services web tout en renforçant leur sécurité. Explorez les fonctionnalités avancées de NGINX Proxy Manager pour optimiser et sécuriser vos applications web avec efficacité.
