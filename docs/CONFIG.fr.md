# Référence de configuration OnAir

OnAir lit ses paramètres depuis `~/Library/Application Support/OnAir/OnAir.cfg`.

La plupart des paramètres sont modifiables via **Préférences** dans l'application.
Les utilisateurs avancés peuvent aussi éditer le fichier `.cfg` directement.
Chaque ligne suit le format `clé:valeur`.

Après modification du `.cfg`, redémarre OnAir pour que les changements prennent
effet.

> ⚠️ **Les identifiants sensibles ne sont pas stockés dans ce fichier.** Ta
> clé API Anthropic est stockée dans le Keychain macOS — le `.cfg` ne contient
> que le placeholder `@keychain`.

---

## Périphérique matériel

Paramètres pour l'enseigne « On Air » physique. OnAir fonctionne entièrement
sans périphérique, mais si tu en as un, configure-le ici.

### `device_type`
- **Type** : enum (`esphome`, `none`, ou vide)
- **Défaut** : `esphome`
- **UI** : Préférences → Périphérique
- **Description** : Type de périphérique. Utilise `esphome` pour une enseigne
  ESPHome (matériel supporté), ou vide / `none` pour fonctionner sans
  périphérique.

### `esphome_name`
- **Type** : chaîne (hostname mDNS)
- **Défaut** : `` (vide)
- **UI** : Préférences → Périphérique
- **Description** : Hostname mDNS de ton périphérique ESPHome, ex.
  `onair-c24d79`. OnAir résout automatiquement vers une adresse IP.
- **Exemple** : `onair-a1b2c3`

### `esphome_ip`
- **Type** : chaîne (adresse IP)
- **Défaut** : `` (vide)
- **UI** : rempli automatiquement quand `esphome_name` se résout
- **Description** : Adresse IP en cache pour le périphérique ESPHome. OnAir
  l'écrase quand la résolution mDNS réussit. Modification manuelle rarement
  nécessaire.
- **Exemple** : `192.168.1.42`

### `esphome_entity_id`
- **Type** : chaîne
- **Défaut** : `onair_switch`
- **UI** : non exposé
- **Description** : ID interne de l'entité exposée par le firmware ESPHome.
  Ne change pas sauf si tu as personnalisé le firmware ESPHome pour utiliser
  un nom d'entité différent.

### `device_skipped`
- **Type** : booléen (`0` ou `1`)
- **Défaut** : `0`
- **UI** : défini automatiquement quand l'utilisateur saute l'étape
  périphérique dans la configuration initiale
- **Description** : Marque que l'utilisateur a explicitement choisi de
  fonctionner sans périphérique. Quand `1`, OnAir ne montre pas
  d'avertissements liés au périphérique.

---

## Service et heures

Contrôle quand OnAir est actif.

### `default_service_state`
- **Type** : booléen (`0` ou `1`)
- **Défaut** : `1`
- **UI** : non exposé
- **Description** : Indique si le service démarre automatiquement (activé) au
  lancement. Quand `0`, l'utilisateur doit démarrer manuellement le
  monitoring depuis la barre de menu.

### `business_hours_enabled`
- **Type** : booléen (`0` ou `1`)
- **Défaut** : `0`
- **UI** : Préférences → Heures
- **Description** : Quand `1`, OnAir surveille les rencontres uniquement
  pendant les heures de travail. En dehors, le périphérique reste éteint.

### `business_hours_start`
- **Type** : heure (`HH:MM`, format 24h)
- **Défaut** : `08:00`
- **UI** : Préférences → Heures
- **Description** : Heure de début de la plage horaire. Utilisée seulement
  quand `business_hours_enabled` vaut `1`.

### `business_hours_end`
- **Type** : heure (`HH:MM`, format 24h)
- **Défaut** : `17:00`
- **UI** : Préférences → Heures
- **Description** : Heure de fin de la plage horaire. Utilisée seulement
  quand `business_hours_enabled` vaut `1`.

### `business_hours_days`
- **Type** : liste séparée par virgules (`mon`, `tue`, `wed`, `thu`, `fri`, `sat`, `sun`)
- **Défaut** : `mon,tue,wed,thu,fri`
- **UI** : Préférences → Heures
- **Description** : Jours de la semaine où les heures de travail s'appliquent.
  L'ordre n'a pas d'importance. Utilisé seulement quand
  `business_hours_enabled` vaut `1`.
- **Exemple** : `mon,wed,fri`

### `week_start_day`
- **Type** : chaîne (`monday` ou `sunday`)
- **Défaut** : `monday`
- **UI** : Préférences → Heures
- **Description** : Premier jour de la semaine dans la vue Statistiques.
  Affecte le regroupement hebdomadaire des données.

---

## Détection des rencontres

Contrôle quelles plateformes OnAir surveille pour détecter les rencontres.

### `monitor_teams`
- **Type** : booléen (`0` ou `1`)
- **Défaut** : `1`
- **UI** : Préférences → Surveillance
- **Description** : Détecte les rencontres Microsoft Teams via l'API Teams.

### `monitor_zoom`
- **Type** : booléen (`0` ou `1`)
- **Défaut** : `1`
- **UI** : Préférences → Surveillance
- **Description** : Détecte les rencontres Zoom.

### `monitor_camera`
- **Type** : booléen (`0` ou `1`)
- **Défaut** : `1`
- **UI** : Préférences → Surveillance
- **Description** : Active OnAir quand n'importe quelle app utilise la caméra
  (FaceTime, Meet, appels via navigateur, etc.).

### `monitor_microphone`
- **Type** : booléen (`0` ou `1`)
- **Défaut** : `1`
- **UI** : Préférences → Surveillance
- **Description** : Active OnAir quand n'importe quelle app utilise le micro.

### `camera_cooldown`
- **Type** : entier (secondes)
- **Défaut** : `3`
- **UI** : non exposé
- **Description** : Temps minimum d'inactivité de la caméra avant qu'OnAir
  considère que la rencontre est terminée. Évite les clignotements quand
  une app relâche brièvement la caméra.

### `microphone_cooldown`
- **Type** : entier (secondes)
- **Défaut** : `3`
- **UI** : non exposé
- **Description** : Temps minimum d'inactivité du micro avant qu'OnAir
  considère que la rencontre est terminée.

---

## Enregistrement des rencontres (Pro)

Ces paramètres ne s'appliquent qu'à OnAir Pro, qui enregistre, transcrit et
résume les rencontres avec l'IA.

### `record_meetings`
- **Type** : booléen (`0` ou `1`)
- **Défaut** : `0`
- **UI** : Préférences → Enregistrement
- **Description** : Interrupteur principal pour l'enregistrement. Quand `1`,
  OnAir capture l'audio de chaque rencontre détectée et produit une
  transcription + un résumé IA.

### `recording_mic_device`
- **Type** : chaîne (nom du périphérique Core Audio)
- **Défaut** : `` (sélection auto)
- **UI** : Préférences → Enregistrement → Microphone
- **Description** : Microphone spécifique pour enregistrer ta voix. Quand vide,
  OnAir utilise le défaut système.
- **Exemple** : `MacBook Pro Microphone`

### `recording_retention_days`
- **Type** : entier (jours)
- **Défaut** : `30`
- **UI** : Préférences → Enregistrement
- **Description** : Combien de temps garder les fichiers audio + transcription
  sur le disque. Utilise `0` pour garder indéfiniment. Les fichiers plus vieux
  sont supprimés automatiquement au lancement d'OnAir.

### `min_recording_seconds`
- **Type** : entier (secondes)
- **Défaut** : `30`
- **UI** : non exposé
- **Description** : Durée minimum d'une rencontre pour qu'OnAir conserve
  l'enregistrement. Les rencontres plus courtes sont jetées (évite de
  sauvegarder des appels accidentels).

### `transcription_segment_minutes`
- **Type** : entier (minutes)
- **Défaut** : `5`
- **UI** : non exposé
- **Description** : Durée des segments audio envoyés à Whisper. Whisper a un
  problème connu avec les longs audios (hallucinations), donc OnAir découpe
  les enregistrements plus longs. Le défaut de 5 minutes est bien testé.

---

## Génération de résumé (Pro)

Configure le moteur IA qui produit les résumés à partir des transcriptions.

### `summary_backend`
- **Type** : enum (`claude`, `ollama`, ou vide)
- **Défaut** : `` (vide — l'utilisateur doit choisir au premier lancement)
- **UI** : Préférences → Enregistrement → Backend
- **Description** : Quel service IA génère les résumés.
  - `claude` : API Anthropic Claude (cloud, nécessite une clé API)
  - `ollama` : instance Ollama locale (privé, nécessite Ollama installé)
  - vide : pas de résumés (transcription seulement)

### `anthropic_api_key`
- **Type** : chaîne (placeholder `@keychain` ou vide)
- **Défaut** : `@keychain`
- **UI** : Préférences → Clé API
- **Description** : **La vraie clé est stockée dans le Keychain macOS**, pas
  dans ce fichier. La valeur `@keychain` indique à OnAir de lire depuis le
  Keychain. Utilisé seulement quand `summary_backend` vaut `claude`.

### `claude_model`
- **Type** : chaîne (identifiant de modèle Anthropic)
- **Défaut** : `claude-haiku-4-5-20251001` (quand non défini ou vide)
- **UI** : non exposé
- **Description** : Quel modèle Claude utiliser pour les résumés. Haiku 4.5
  est le défaut recommandé (bonne qualité, peu coûteux). Sonnet 4.5 produit
  des résumés légèrement différents à ~3x le coût.
- **Exemple** : `claude-sonnet-4-5-20250929`

### `ollama_url`
- **Type** : URL
- **Défaut** : `http://localhost:11434`
- **UI** : Préférences → Enregistrement → URL Ollama
- **Description** : URL de base du serveur Ollama. Utilise une URL distante
  si Ollama tourne sur une machine plus puissante de ton réseau.
- **Exemples** :
  - `http://localhost:11434`
  - `http://homeserver.local:11434`
  - `http://192.168.1.50:11434`

### `ollama_model`
- **Type** : chaîne (nom du modèle Ollama)
- **Défaut** : `qwen3:8b`
- **UI** : Préférences → Enregistrement → Modèle Ollama
- **Description** : Quel modèle Ollama doit utiliser. Les petits modèles (8B)
  sont rapides mais produisent des résumés moins détaillés. Les gros modèles
  (32B+) demandent plus de RAM.

### `ollama_num_ctx`
- **Type** : entier (tokens)
- **Défaut** : `16384`
- **UI** : non exposé (paramètre avancé)
- **Description** : Taille de la fenêtre de contexte pour Ollama. Des valeurs
  plus élevées permettent à de plus longues rencontres de tenir en une seule
  requête, mais consomment plus de RAM. La fonction de chunking d'OnAir
  découpe automatiquement les longues rencontres, donc tu n'as rarement à
  modifier ça. Augmente seulement si tu as un GPU puissant et veux moins
  de chunks.
- **Recommandé** :
  - 8 Go RAM : `4096` ou `8192`
  - 16 Go RAM : `8192` ou `16384`
  - 32+ Go RAM : `32768`

### `summary_language`
- **Type** : chaîne (`French`, `English`, `Auto`)
- **Défaut** : `Auto`
- **UI** : Préférences → Enregistrement → Langue du résumé
- **Description** : Langue du résumé généré. `Auto` détecte depuis la
  transcription.

---

## Raccourcis et mode Concentration

Intègre OnAir avec Raccourcis macOS et les modes Concentration.

### `focus_name`
- **Type** : chaîne (nom du mode Concentration)
- **Défaut** : `` (vide)
- **UI** : Préférences → Concentration
- **Description** : Nom du mode Concentration macOS à activer quand une
  rencontre démarre. Laisse vide pour désactiver l'intégration.
- **Exemple** : `Work`

### `shortcut_on_call_start`
- **Type** : chaîne (nom de raccourci)
- **Défaut** : `` (vide)
- **UI** : Préférences → Raccourcis
- **Description** : Nom d'un raccourci de l'app Raccourcis à exécuter quand
  une rencontre démarre. Laisse vide pour ignorer.

### `shortcut_on_call_start_confirm`
- **Type** : booléen (`0` ou `1`)
- **Défaut** : `0`
- **UI** : Préférences → Raccourcis
- **Description** : Affiche un dialogue de confirmation avant d'exécuter
  `shortcut_on_call_start`.

### `shortcut_on_call_end`
- **Type** : chaîne (nom de raccourci)
- **Défaut** : `` (vide)
- **UI** : Préférences → Raccourcis
- **Description** : Nom d'un raccourci de l'app Raccourcis à exécuter quand
  une rencontre se termine.

### `shortcut_on_call_end_confirm`
- **Type** : booléen (`0` ou `1`)
- **Défaut** : `0`
- **UI** : Préférences → Raccourcis
- **Description** : Affiche un dialogue de confirmation avant d'exécuter
  `shortcut_on_call_end`.

---

## Webhooks

Envoie des requêtes HTTP au démarrage / à la fin des rencontres. Utile pour
intégrer avec Home Assistant, IFTTT, ou de l'automatisation custom.

### `webhook_url`
- **Type** : URL
- **Défaut** : `` (vide)
- **UI** : Préférences → Webhooks
- **Description** : URL où envoyer un POST lors des changements d'état des
  rencontres. Laisse vide pour désactiver les webhooks.

### `webhook_confirm`
- **Type** : booléen (`0` ou `1`)
- **Défaut** : `0`
- **UI** : Préférences → Webhooks
- **Description** : Affiche un dialogue de confirmation avant d'envoyer le
  webhook.

---

## Maintenance

OnAir migre automatiquement le `.cfg` au démarrage :

- **Clés manquantes** : ajoutées avec leurs valeurs par défaut.
- **Clés obsolètes** (legacy, plus utilisées par le code) : retirées
  silencieusement.

Les deux événements sont loggés dans `~/Library/Logs/OnAir.log`.

Si tu veux repartir à zéro, quitte OnAir et supprime `OnAir.cfg`. Au prochain
lancement, OnAir le recréera avec tous les défauts.
