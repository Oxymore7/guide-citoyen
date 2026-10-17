# Mettre en ligne le Guide du citoyen

Ce dossier contient le site complet du Guide du citoyen, prêt à déposer chez l'hébergeur Netlify :

- `site/` : le site lui-même (le guide, la recherche, « Où aller », l'installation sur téléphone et la lecture hors connexion) ;
- `netlify/functions/ask.js` : l'assistant qui répond aux questions (il reste éteint tant qu'aucune clé n'est configurée) ;
- `netlify/functions/guide.json` : le contenu du guide utilisé par l'assistant ;
- `netlify.toml` : les réglages pour Netlify.

Le site fonctionne en deux temps : vous pouvez le mettre en ligne tout de suite **sans assistant** (gratuit), puis activer l'assistant plus tard.

---

## Étape 1 : mettre le guide en ligne (gratuit, 15 minutes)

L'hébergement par simple glisser-déposer ne prend pas en charge la fonction de l'assistant. On passe donc par GitHub, gratuit lui aussi, ce qui permettra plus tard d'activer l'assistant sans rien refaire.

1. **Créez un compte GitHub** sur https://github.com (gratuit).
2. **Créez un dépôt** : bouton « New repository », nom `guide-citoyen`, visibilité « Public » ou « Private », puis « Create repository ».
3. **Déposez les fichiers** : sur la page du dépôt, cliquez sur « uploading an existing file », glissez **tout le contenu** de ce dossier (le dossier `site`, le dossier `netlify`, le fichier `netlify.toml` et ce fichier), puis « Commit changes ».
4. **Créez un compte Netlify** sur https://www.netlify.com en choisissant « Sign up with GitHub ».
5. Dans Netlify : « Add new site », puis « Import an existing project », puis « GitHub », et choisissez le dépôt `guide-citoyen`. Laissez les réglages proposés (ils viennent du fichier `netlify.toml`) et cliquez sur « Deploy ».
6. Au bout d'une minute, le site est en ligne à une adresse du type `https://guide-citoyen-xxxx.netlify.app`. Dans « Site configuration », puis « Change site name », vous pouvez choisir par exemple `guide-citoyen-congo`.

À ce stade, la page « Poser une question » affiche les passages du guide correspondant à la question, sans l'assistant.

**Installer sur téléphone :** ouvrez le site dans Chrome sur Android, puis menu « ⋮ » et « Ajouter à l'écran d'accueil » (sur iPhone : Safari, bouton Partager, « Sur l'écran d'accueil »). Le guide reste ensuite lisible sans connexion.

---

## Étape 2 : activer l'assistant (payant à l'usage)

1. **Créez un compte développeur Anthropic** sur https://console.anthropic.com au nom de l'association.
2. Dans « Billing », ajoutez un moyen de paiement et créditez le compte.
3. **Fixez un plafond de dépenses mensuel** dans les réglages de limites (« Limits ») de la console. C'est la protection la plus importante : au-delà, l'assistant s'arrête au lieu de vous coûter davantage.
4. Dans « API Keys », cliquez sur « Create Key » et copiez la clé (elle commence par `sk-ant-`). Ne la publiez jamais et ne l'envoyez à personne.
5. Dans Netlify : « Site configuration », puis « Environment variables », puis « Add a variable » :
   - nom : `ANTHROPIC_API_KEY`, valeur : la clé copiée ;
   - facultatif : `MAX_PAR_HEURE` (nombre de questions autorisées par personne et par heure, 15 par défaut) ;
   - facultatif : `CLAUDE_MODEL` (modèle utilisé, `claude-sonnet-5-5` par défaut ; `claude-haiku-5-5` coûte moins cher mais répond moins finement).
6. « Deploys », puis « Trigger deploy » et « Deploy site ». L'assistant est actif après le redéploiement.

**Suivre les dépenses :** la console Anthropic affiche la consommation jour par jour. Regardez-la chaque semaine au début.

---

## Mettre à jour le guide

Quand le guide change, le site doit être régénéré : demandez à Claude de régénérer le dossier à partir de la nouvelle version du guide, puis remplacez sur GitHub les fichiers `site/index.html` et `netlify/functions/guide.json` (bouton « Add file », puis « Upload files »). Netlify republie automatiquement.

Pensez aussi à changer la ligne `const VERSION = ...` en haut du fichier `site/sw.js` (par exemple avec la date du jour), pour que les téléphones qui ont installé le guide reçoivent la nouvelle version.

---

## Nom de domaine (facultatif)

Pour une adresse comme `guidecitoyen.org` ou `guidecitoyen.cg`, achetez le nom chez un registraire (environ une dizaine de dollars par an pour un `.org`), puis dans Netlify : « Domain management », puis « Add a domain », et suivez les indications. Le certificat de sécurité (https) est fourni gratuitement par Netlify.

---

## Bon à savoir

- **Confidentialité :** le site n'enregistre ni les questions ni les numéros. Les questions envoyées à l'assistant passent par Anthropic pour produire la réponse. Netlify garde des journaux techniques de courte durée.
- **Avertissement :** le site rappelle qu'il donne une information générale et ne remplace pas un avocat. Gardez cette mention.
- **Le même assistant pour WhatsApp :** la fonction `ask.js` pourra être réutilisée telle quelle par un robot WhatsApp ; seule l'entrée change.
