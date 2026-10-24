# Reporting Cargill

Appli de comptes rendus d'intervention pour Dimitri Van Calster (Cargill).
Elle remplace le fichier Excel « Reporting_Cargill » : une saisie par intervention,
regroupée par semaine et par jour, avec export PDF et envoi par mail au n+1.

## Contenu du dossier

| Fichier | Rôle |
|---|---|
| `index.html` | L'appli entière (écrans, calculs, PDF) |
| `manifest.webmanifest` | Permet de l'installer sur l'écran d'accueil du téléphone |
| `sw.js` | Garde l'appli disponible sans réseau |
| `icons/` | Icônes de l'appli |

## 1. Mettre l'appli en ligne (GitHub Pages)

1. Sur GitHub, crée un nouveau dépôt (par exemple `reporting-cargill`), en public.
2. « Add file » → « Upload files » : glisse **tout le contenu** du dossier (y compris le dossier `icons`), puis « Commit changes ».
3. « Settings » → « Pages » → Source : « Deploy from a branch », branche `main`, dossier `/ (root)` → « Save ».
4. Au bout d'une minute, l'adresse apparaît en haut de la page Pages : `https://TON-PSEUDO.github.io/reporting-cargill/`.

Le dépôt ne contient aucune donnée : les comptes rendus restent sur le téléphone de Dimitri.

## 2. L'installer sur le téléphone de Dimitri

- **iPhone (Safari)** : ouvrir l'adresse → bouton Partager → « Sur l'écran d'accueil ».
- **Android (Chrome)** : ouvrir l'adresse → menu ⋮ → « Installer l'application » (ou « Ajouter à l'écran d'accueil »).

Ensuite, dans l'appli : **Réglages** → renseigner le nom et l'adresse mail du n+1.

## 3. Utilisation au quotidien

1. Accueil → la semaine en cours → toucher un jour (ou « Nouveau compte rendu » pour aujourd'hui).
2. « Créer nouveau compte rendu d'intervention » :
   - **Work order** : photographier le WO ou choisir le PDF → le n° de WO, le n° d'avis, le bâtiment, la localisation, l'équipement et le travail demandé se remplissent tout seuls (à vérifier d'un coup d'œil).
   - **Date** au calendrier, **horaires** début/fin (ou raccourcis Journée / Matin / Après-midi). La pause déjeuner est déduite.
   - **Commentaires** : bouton micro pour dicter. Si la dictée n'est pas disponible, le micro du clavier du téléphone fonctionne aussi.
   - **Temps passé** : calculé d'après les horaires, modifiable dans la liste.
   - **Cumul sur ce WO** : calculé automatiquement en additionnant toutes les interventions qui portent le même n° de WO. Modifiable si une partie du travail a été faite avant d'utiliser l'appli ; les interventions suivantes repartent de ce cumul.
   - **Photos** : appareil photo ou galerie.
3. En fin de journée, sur la page du jour : « PDF de la journée · envoi au n+1 ».
4. En fin de semaine, sur l'accueil : « Compte rendu de la semaine ».

Le PDF reprend la présentation du reporting Excel actuel (date, horaires, WO, intervention, observations, temps, cumul WO, total jour et total semaine), avec les photos en annexe.

Si le téléphone se met en veille pendant la saisie, le brouillon est gardé et proposé à la réouverture.

## 4. Envoi par mail

**Par défaut (rien à configurer)** : « Envoyer par mail » ouvre la liste de partage du téléphone. On choisit Gmail : le PDF, l'objet et le message sont déjà remplis, il reste à mettre l'adresse du n+1 et à envoyer.

**Envoi direct depuis Gmail (optionnel, environ 15 minutes de réglage, une seule fois)** : l'appli envoie elle-même le mail avec le PDF depuis la boîte Gmail de Dimitri, destinataire déjà rempli.

1. Aller sur https://console.cloud.google.com et créer un projet (« Reporting Cargill »).
2. « API et services » → « Bibliothèque » → chercher **Gmail API** → « Activer ».
3. « API et services » → « Écran de consentement OAuth » : type **Externe**, nom de l'appli, ton adresse mail. Dans « Utilisateurs test », ajouter **l'adresse Gmail de Dimitri**. Laisser l'appli en mode « Test ».
4. « API et services » → « Identifiants » → « Créer des identifiants » → « ID client OAuth » → type **Application Web**.
   Dans « Origines JavaScript autorisées », ajouter `https://TON-PSEUDO.github.io` (sans le nom du dépôt, sans `/` à la fin).
5. Copier l'**ID client** (il finit par `.apps.googleusercontent.com`).
6. Dans `index.html`, tout en haut du script, coller l'ID entre les guillemets :
   `googleClientId: '1234-abcd.apps.googleusercontent.com'`
   puis renvoyer `index.html` sur GitHub.

Au premier envoi, Google affiche « Google n'a pas validé cette application » : c'est normal pour une appli personnelle en mode test. Toucher « Continuer », puis autoriser l'envoi de mails. L'appli ne demande que le droit d'**envoyer** des mails, pas de les lire.

## 5. Sauvegarde

Les données sont stockées dans le navigateur du téléphone. Si on efface les données de Safari/Chrome ou qu'on change de téléphone, elles disparaissent.
Réglages → « Sauvegarder mes données » crée un fichier (photos comprises) à garder dans Drive ou par mail ; « Restaurer une sauvegarde » le recharge.

## 6. Bon à savoir

- La première ouverture, le premier PDF et la première lecture de WO par photo ont besoin de réseau (chargement des outils). Ensuite tout fonctionne hors ligne, sauf l'envoi du mail.
- La lecture d'un WO en PDF est quasi instantanée. Par photo, compter 10 à 30 secondes selon le téléphone ; une photo nette, à plat et bien éclairée donne les meilleurs résultats.
- La lecture automatique repère les intitulés courants d'un ordre de travail (« Ordre », « Avis », « Poste technique », « Équipement », « Description », « Bât. »…). Si le format Cargill utilise d'autres intitulés, quelques lignes de `parseWO` dans `index.html` sont à ajuster.
