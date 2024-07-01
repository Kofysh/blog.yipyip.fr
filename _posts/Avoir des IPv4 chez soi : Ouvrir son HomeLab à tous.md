# Avoir des IPv4 chez soi : Ouvrir son HomeLab à tous

**Description :** Comment j'ai réussi à installer facilement des IP Failover chez moi !

**Résumé :** Lorsqu'on héberge chez soi, on peut rapidement être limité par une seule IP. Découvrez comment obtenir plusieurs IP Failover pour optimiser votre HomeLab.

**Tags :** [tunnel, hostmyservers, hms, ipfailover, ipfo, wireguard]

---

## Introduction

J'ai un serveur chez moi sur lequel j'ai installé [Proxmox](https://www.proxmox.com/en/proxmox-ve), un hyperviseur qui me permet de créer plusieurs machines virtuelles. Ces machines virtuelles hébergent divers services pour moi et pour d'autres utilisateurs.

Cependant, avec une seule IP résidentielle, je suis rapidement limité par le nombre de ports disponibles et par les services que je souhaite faire tourner dans des conditions optimales. J'ai donc cherché un moyen d'obtenir des IP dédiées, protégées par un Anti-DDOS et à coût réduit. Ne trouvant pas de solution satisfaisante, j'ai décidé de créer la mienne.

## Les différentes approches

Nous allons aborder plusieurs approches pour obtenir des IP chez soi et publier son HomeLab. Avec la pénurie actuelle d'IPv4, il est important de ne pas commander massivement des adresses inutilisées, privant ainsi d'autres utilisateurs. Voici les approches traitées dans cette documentation :

1. **Méthode originale** : Associer à n'importe quelle machine une adresse IP publique via un profil WireGuard dédié.
2. **Méthode alternative** : Utiliser un tunnel VPN pour assigner des IP publiques.

![Schéma explicatif](https://img.creeper.fr/Kiba9/tuQUwUmU80.png/raw)

## Génération de certificats SSL

Les certificats SSL gratuits permettent d'avoir l'HTTPS sur vos services. Voici les prérequis :

- Un nom de domaine
- Le contrôle de la zone DNS (Cloudflare est recommandé)
- Une règle DNS A pointant vers l'IP principale du VPS HMS

![Préparation des certificats SSL](https://img.creeper.fr/Kiba9/QicegAwo63.png/raw)

### Étape 1 : Générer un certificat SSL

1. Accédez à la rubrique **SSL Certificates** via la barre de navigation.
   
   ![SSL Certificates](https://img.creeper.fr/Kiba9/bIKImOkU61.png/raw)
   
2. Cliquez sur **Add SSL Certificate**.
   
   ![Ajouter un certificat SSL](https://img.creeper.fr/Kiba9/jOyikAPi20.png/raw)

3. Entrez le nom de domaine du service que vous souhaitez sécuriser. Pour un sous-domaine, utilisez par exemple `*.votredomaine.fr` et suivez le DNS Challenge pour valider le certificat.

   ![DNS Challenge](https://img.creeper.fr/Kiba9/KiLOdELU62.png/raw)

   ![Validation du certificat](https://img.creeper.fr/Kiba9/RICOWutO99.png/raw)

### Étape 2 : Créer le service associé

1. Une fois le certificat généré, accédez à la rubrique **Hosts**.
   
   ![Hosts](https://img.creeper.fr/Kiba9/YAFiSutI96.png/raw)

2. Cliquez sur **Add Proxy Host**.
   
   ![Ajouter un Proxy Host](https://img.creeper.fr/Kiba9/vikEQuVe99.png/raw)

3. Configurez les paramètres :
   - **Domain Names** : `sousdomaine.votresite.fr`
   - **Scheme** : `http`
   - **Forward IP** : L'IP locale (par exemple, `http://ip:8123`)
   - **Forward Port** : `8123`
   
   Activez les options "Block Common Exploits" et "Websockets Support".

   ![Configuration du Proxy Host](https://img.creeper.fr/Kiba9/QewiwebO93.png/raw)

4. Dans la rubrique SSL, sélectionnez le certificat SSL généré précédemment, et cochez "Force SSL" et "HTTP/2 Support".

   ![Configuration SSL](https://img.creeper.fr/Kiba9/pEmICuKI58.png/raw)

5. Sauvegardez en cliquant sur **Save**.

   ![Sauvegarder la configuration](https://img.creeper.fr/Kiba9/lUTuVEzU83.png/raw)

---

Cette documentation vous guide à travers les étapes pour obtenir plusieurs IP Failover et sécuriser vos services avec des certificats SSL. En suivant ces instructions, vous pouvez optimiser votre HomeLab et héberger plusieurs services avec une meilleure gestion des ressources réseau. Pour toute question ou assistance supplémentaire, n'hésitez pas à consulter les ressources et communautés en ligne.
