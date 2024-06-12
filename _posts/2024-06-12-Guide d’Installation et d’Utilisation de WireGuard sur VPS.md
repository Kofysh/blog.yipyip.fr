WireGuard est un protocole VPN utilisant l'UDP. Il est compatible avec de nombreuses plateformes, extrêmement léger, très facile à déployer et beaucoup plus performant que ses concurrents tout en restant sécurisé. C'est un petit nouveau dans le domaine de l'open-source et il a fait ses preuves ces deux dernières années.

## Avantages de WireGuard

- **Compatibilité étendue** : Fonctionne sur diverses plateformes.
- **Légèreté** : Très faible consommation de ressources.
- **Facilité de déploiement** : Installation simple et rapide.
- **Performances élevées** : Meilleur débit par rapport aux autres VPN.
- **Sécurité** : Protège vos données efficacement.

## Dépendance à la Qualité du Réseau

La qualité de l'interconnexion dépendra de votre réseau. WireGuard ne nécessite pas une bonne connexion internet et ne réduira pas votre débit. Cependant, il faudra ajouter le ping entre vous → HMS et HMS → vous.

![Performance Comparaison](https://korben.info/app/uploads/2020/02/bench.png)

## Prérequis

Pour la suite de ce guide, il est nécessaire que vous utilisiez Debian 12 ou une version ultérieure.

## Grille Tarifaire d'HMS (au 29/12/2022)

| Modèle     | SSD-1 | SSD-2 | SSD-4 | SSD-8 | SSD-12 | SSD-12' |
|------------|-------|-------|-------|-------|--------|---------|
| Prix mensuel sans engagement (TTC) | 2,99€ | 5,99€ | 9,99€ | 19,99€ | 29,99€ | 39,99€ |
| Bande passante | 250 Mbit/s | 500 Mbit/s | 800 Mbit/s | 1 Gbit/s | 2 Gbit/s | 3 Gbit/s |
| vCore(s) | 1 | 2 | 4 | 8 | 12 | 12 |
| Mémoire RAM | 2 Go | 4 Go | 8 Go | 16 Go | 24 Go | 32 Go |
| Stockage SSD | 20 Go | 40 Go | 60 Go | 120 Go | 160 Go | 200 Go |

Ces offres ne disposent pas de limites d'IP Failover. Vous pouvez en prendre autant que vous le souhaitez, elles coûtent 1,99€ à vie.

## Commande d'IP Additionnelle

Pour commander une IP additionnelle, une fois le VPS livré, rendez-vous dans l'Espace Client : **Votre VPS → Configuration → Commander une Nouvelle IP**. Une fois la commande passée, un mail de confirmation vous sera envoyé.

## Configuration de WireGuard

### Étape 1 : Installation

1. Mettez à jour votre système :
    ```bash
    sudo apt update && sudo apt upgrade
    ```
2. Installez WireGuard :
    ```bash
    sudo apt install wireguard
    ```

### Étape 2 : Configuration du Serveur

1. Créez les clés privées et publiques :
    ```bash
    wg genkey | tee /etc/wireguard/privatekey | wg pubkey > /etc/wireguard/publickey
    ```

2. Configurez le fichier `/etc/wireguard/wg0.conf` :
    ```ini
    [Interface]
    PrivateKey = votre_clé_privée
    Address = 10.0.0.1/24
    ListenPort = 51820

    [Peer]
    PublicKey = clé_publique_du_client
    AllowedIPs = 10.0.0.2/32
    ```

### Étape 3 : Démarrage de WireGuard

1. Démarrez le service :
    ```bash
    sudo wg-quick up wg0
    ```

2. Activez le service au démarrage :
    ```bash
    sudo systemctl enable wg-quick@wg0
    ```

### Étape 4 : Configuration du Client

1. Installez WireGuard sur le client :
    ```bash
    sudo apt install wireguard
    ```

2. Configurez le fichier `/etc/wireguard/wg0.conf` :
    ```ini
    [Interface]
    PrivateKey = votre_clé_privée
    Address = 10.0.0.2/24

    [Peer]
    PublicKey = clé_publique_du_serveur
    Endpoint = adresse_du_serveur:51820
    AllowedIPs = 0.0.0.0/0
    ```

3. Démarrez le service :
    ```bash
    sudo wg-quick up wg0
    ```

## Remerciements

- [@Aven678](https://github.com/Aven678) : Pour la simplification de la gestion des IPs et la création de profils.
- [@DrKnaw](https://github.com/DrKnaw) : Pour avoir corrigé des bugs initiaux.
- [@Mael](https://github.com/maelmagnien) : Pour les tests et la validation du tutoriel.
- @Twistky, @Diggyworld, @titin : Pour l'aide précieuse et les solutions aux problèmes rencontrés.

Merci à tous ceux qui ont contribué à améliorer cette documentation ou m'ont remercié pour celle-ci.

## Liens Utiles

- [WireGuard Documentation](https://github.com/pirate/wireguard-docs)

Merci d'avoir suivi cette documentation, j'espère qu'elle vous sera très utile.
