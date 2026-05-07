# Politique de confidentialité d'OnAir

**Dernière mise à jour** : 6 mai 2026
**Date d'entrée en vigueur** : 6 mai 2026

## Résumé en langage clair

OnAir est conçu avec la confidentialité comme priorité. Nous l'avons conçu
pour garder vos rencontres sur votre Mac et minimiser ce qui quitte votre
ordinateur.

**Ce qui reste sur votre Mac** :
- Tous les enregistrements audio de vos rencontres
- Les transcriptions (générées localement par OpenAI Whisper sur votre Mac)
- Les métadonnées des rencontres (heures de début/fin, durées, statistiques)
- Vos paramètres et préférences

**Ce qui peut quitter votre Mac (uniquement si vous le choisissez)** :
- Le texte transcrit envoyé à l'API Claude d'Anthropic pour générer un
  résumé (uniquement si vous sélectionnez Claude comme backend de résumé
  et configurez votre propre clé API)

**Ce qui quitte toujours votre Mac** :
- Vérifications anonymes de mises à jour vers GitHub (pour savoir si une
  nouvelle version est disponible)

**Ce qui ne quitte jamais votre Mac** :
- Les enregistrements audio (toujours locaux, même avec le backend Claude)
- Vos clés API (stockées dans le Keychain macOS, jamais transmises à nous)
- Vos informations personnelles

**Nous ne collectons aucune analytique, télémétrie ou donnée d'usage.** Nous
n'avons pas de serveurs qui stockent vos données. Nous ne vous traçons pas.

Si vous utilisez le backend Ollama au lieu de Claude, **rien concernant vos
rencontres ne quitte jamais votre Mac**.

---

## 1. Qui nous sommes

OnAir est édité par :

**Solucor Inc.**
Québec, Canada
Contact : [À COMPLÉTER avec un email de contact, ex: privacy@onair.solucor.com]

Cette Politique de confidentialité explique comment OnAir gère vos données
personnelles, en conformité avec :

- **Loi 25 du Québec** (Loi sur la protection des renseignements personnels
  dans le secteur privé)
- **LPRPDE du Canada** (Loi sur la protection des renseignements personnels
  et les documents électroniques)
- **RGPD de l'UE** (Règlement général sur la protection des données)

---

## 2. Quelles données OnAir traite

### 2.1 Localement sur votre Mac

OnAir stocke les données suivantes sur votre Mac, dans
`~/Library/Application Support/OnAir/` et le Keychain macOS :

- **Enregistrements audio** : fichiers WAV des rencontres que vous choisissez
  d'enregistrer (Pro uniquement)
- **Transcriptions** : texte généré par OpenAI Whisper exécuté localement
- **Résumés IA** : résumés structurés générés par Claude ou Ollama
- **Métadonnées des rencontres** : horodatages, durées, plateformes
  détectées, statistiques
- **Préférences de l'app** : vos paramètres, dont les options de
  surveillance actives
- **Clé API Anthropic** : stockée chiffrée dans le Keychain macOS
  (uniquement si vous utilisez le backend Claude)

Ces fichiers sont protégés par votre compte utilisateur macOS et votre
Keychain. Nous n'y avons pas accès.

### 2.2 Données qui quittent votre Mac

**OnAir n'a pas de serveurs**. Nous ne recevons aucune de vos données.

Les données suivantes quittent votre Mac selon votre configuration :

#### Lorsque vous générez un résumé avec Claude (optionnel)

Si vous configurez OnAir pour utiliser l'API Claude d'Anthropic, OnAir envoie
à Anthropic :

- Le texte transcrit de la rencontre (en clair, via HTTPS)
- Une instruction (prompt) demandant un résumé structuré
- Votre clé API Anthropic (dans l'en-tête de la requête, comme exigé par
  l'API)

La transcription est envoyée **sous forme de texte**, jamais d'audio.
Anthropic traite la requête et retourne un résumé. Nous vous renvoyons à la
[Politique de confidentialité d'Anthropic](https://www.anthropic.com/legal/privacy)
pour les détails sur leur traitement de ces données.

Vous entrez dans une relation directe avec Anthropic lorsque vous configurez
votre clé API. OnAir n'est pas partie à cette relation.

#### Lorsque vous générez un résumé avec Ollama (optionnel)

Si vous choisissez Ollama comme backend, OnAir envoie la transcription au
serveur Ollama que vous avez configuré. Par défaut, c'est
`http://localhost:11434` (votre propre Mac), donc **rien ne quitte votre
ordinateur**.

Si vous configurez Ollama pour pointer vers un serveur distant (ex. une
autre machine sur votre réseau), la transcription y est envoyée. C'est
votre décision et hors du périmètre d'OnAir.

#### Lorsqu'OnAir vérifie les mises à jour

OnAir vérifie périodiquement GitHub pour de nouvelles versions (en utilisant
le framework Sparkle). Cela consiste en une requête HTTPS GET vers
`https://github.com/OnAirApp/OnAir-releases/...` pour télécharger un petit
fichier XML listant les versions disponibles.

**Ce que GitHub voit** :
- Votre adresse IP (visible dans leurs logs serveur)
- Votre User-Agent (ex. « Sparkle/2.x macOS/14.x »)
- Le fait qu'une installation OnAir a vérifié les mises à jour

**Ce que GitHub ne voit pas** :
- Aucun identifiant unique vous concernant
- Aucune donnée sur vos rencontres ou votre usage

Nous ne transmettons aucune information de profil système (le
`sendsSystemProfile` de Sparkle est désactivé).

Vous pouvez désactiver les vérifications de mise à jour dans Préférences
Système macOS → Réseau si vous souhaitez empêcher cela.

---

## 3. Combien de temps nous conservons vos données

Comme nous n'avons pas de serveurs, **nous ne conservons aucune de vos
données**. Vos données vivent sur votre Mac, sous votre contrôle.

La rétention locale d'OnAir est configurable :

- **Enregistrements audio** : supprimés selon `recording_retention_days`
  (30 jours par défaut, configurable jusqu'à suppression immédiate via
  Préférences)
- **Transcriptions et résumés** : conservés indéfiniment dans la base de
  données locale d'OnAir jusqu'à ce que vous les supprimiez via la fenêtre
  Statistiques
- **Paramètres** : conservés jusqu'à la désinstallation d'OnAir ou la
  suppression du fichier de configuration

Si vous désinstallez OnAir, vous pouvez supprimer toutes les données
manuellement :

```
~/Library/Application Support/OnAir/
~/Library/Logs/OnAir.log
```

Et retirer la clé API du Keychain via Keychain Access.app (rechercher
« com.solucor.OnAir »).

---

## 4. Tiers

OnAir utilise ces services tiers. Nous ne sommes pas responsables de leur
gestion des données, mais nous les listons ici par souci de transparence :

### Anthropic (optionnel)
**Quand** : uniquement si vous configurez le backend Claude
**Données envoyées** : texte transcrit, votre clé API
**Pourquoi** : pour générer le résumé de la rencontre
**Leur politique** : https://www.anthropic.com/legal/privacy

### GitHub (toujours, pour les mises à jour)
**Quand** : au lancement de l'app et périodiquement
**Données envoyées** : adresse IP, User-Agent
**Pourquoi** : pour vérifier si une nouvelle version d'OnAir est disponible
**Leur politique** : https://docs.github.com/fr/site-policy/privacy-policies

### Apple (au niveau du système d'exploitation)
**Quand** : en continu, dans le cadre de macOS
**Données** : macOS gère la capture audio, le Keychain, le système de fichiers
**Leur politique** : https://www.apple.com/legal/privacy/fr-ca/

### Ollama (optionnel, uniquement si configuré à distance)
**Quand** : uniquement si vous pointez Ollama vers un serveur distant
**Données envoyées** : texte transcrit
**Pourquoi** : pour générer le résumé de la rencontre
**Leur politique** : dépend du serveur que vous avez configuré

---

## 5. Vos droits

Selon votre lieu de résidence, vous disposez des droits suivants :

### Droit d'accès
Vous avez déjà un accès complet à vos données — elles sont sur votre Mac.
Vous pouvez parcourir les fichiers dans
`~/Library/Application Support/OnAir/` et consulter les résumés dans la
fenêtre Statistiques d'OnAir.

### Droit de rectification
Vous pouvez modifier les résumés directement dans l'interface OnAir
(bouton Modifier dans la fenêtre Notes de rencontre). Les fichiers audio
et les transcriptions sont en lecture seule par design (pour préserver
leur intégrité), mais vous pouvez les supprimer.

### Droit à l'effacement
Vous pouvez supprimer des données à tout moment :
- **Rencontres individuelles** : dans la fenêtre Statistiques, sélectionnez
  une rencontre et supprimez-la
- **Toutes les données** : quittez OnAir et supprimez
  `~/Library/Application Support/OnAir/`
- **Clé API** : Préférences → Clé API → Retirer, ou supprimer l'entrée du
  Keychain Access

### Droit à la portabilité
Les résumés de rencontres peuvent être copiés/exportés via la fenêtre
Notes de rencontre (bouton Copier). La base de données sous-jacente est
un fichier SQLite à `~/Library/Application Support/OnAir/meetings.sqlite`
et peut être ouvert avec n'importe quel outil SQLite.

### Droit d'opposition / retrait du consentement
Vous pouvez cesser d'utiliser OnAir à tout moment. Il n'y a pas de compte
à supprimer puisqu'il n'en a jamais existé.

### Droit de déposer une plainte
- **Au Québec** : porter plainte auprès de la Commission d'accès à
  l'information du Québec (https://www.cai.gouv.qc.ca/)
- **Au Canada** : porter plainte auprès du Commissariat à la protection
  de la vie privée du Canada (https://www.priv.gc.ca/)
- **Dans l'UE** : contactez votre autorité nationale de protection des
  données (liste sur https://edpb.europa.eu/about-edpb/about-edpb/members_en)

Pour exercer l'un de ces droits directement avec nous, contactez :
[À COMPLÉTER avec un email de contact]

---

## 6. Sécurité

OnAir utilise les fonctions de sécurité natives de macOS :

- **Keychain** : votre clé API Anthropic est stockée dans le Keychain
  macOS, chiffrée par votre système d'exploitation
- **HTTPS** : toutes les requêtes réseau (API Anthropic, vérifications
  GitHub) utilisent le chiffrement TLS
- **Signature de code** : OnAir est signé avec un Developer ID Apple et
  notarisé, empêchant les modifications malveillantes
- **Sandboxing** : OnAir s'exécute comme une application macOS standard
  avec les permissions que vous accordez (microphone, détection caméra,
  accessibilité pour la détection des rencontres)

Comme nous ne stockons aucune de vos données nous-mêmes, nous ne pouvons
pas les perdre, les divulguer ou les mal gérer.

---

## 7. Enfants

OnAir n'est pas destiné aux enfants de moins de 16 ans. Nous ne collectons
pas sciemment de données auprès d'enfants. Si vous êtes parent ou tuteur
et croyez que votre enfant a utilisé OnAir, vous pouvez supprimer toutes
les données OnAir de votre Mac comme décrit en section 5.

---

## 8. Modifications de cette politique

Nous pouvons mettre à jour cette Politique de confidentialité. La date
« Dernière mise à jour » en haut de ce document indique quand. Les
changements importants seront communiqués via :

- Une note dans les notes de version d'OnAir
- Une date mise à jour en haut de cette page

L'utilisation continue d'OnAir après une modification de la politique
signifie que vous acceptez la politique mise à jour. Vous pouvez toujours
cesser d'utiliser OnAir si vous n'êtes pas d'accord.

---

## 9. Contact

Pour toute question liée à la confidentialité :

**Solucor Inc.**
Courriel : [À COMPLÉTER]
Adresse postale : [À COMPLÉTER]

Pour les requêtes spécifiques au RGPD (résidents UE), veuillez mentionner
« Demande RGPD » dans votre objet.

---

## 10. Avis de non-responsabilité

OnAir est fourni tel quel. Nous faisons de notre mieux pour protéger votre
confidentialité par notre architecture (tout local), mais nous ne pouvons
pas garantir une sécurité parfaite. Vous êtes responsable de :

- Sécuriser votre Mac (mot de passe de session, FileVault, etc.)
- Garder votre clé API Anthropic en sécurité
- Examiner les politiques des tiers (Anthropic, Ollama si distant, etc.)
- Respecter les lois applicables lors de l'enregistrement de rencontres
  (certaines juridictions exigent le consentement de tous les participants)

**Vous êtes responsable d'obtenir le consentement des participants à la
rencontre avant l'enregistrement.** OnAir ne gère pas cela pour vous. Les
lois varient selon les juridictions — vérifiez les vôtres.