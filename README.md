# LAB 9 : Analyse de surface d'attaque Android avec Drozer (audit défensif en environnement autorisé)
analyste : souaid med amine

Ce projet consiste à réaliser une analyse de sécurité d’une application Android à l’aide de l’outil Drozer. L’objectif principal est 

d’identifier les composants exposés (Activities, Services, Broadcast Receivers et Content Providers), d’analyser leur niveau de 

protection et d’évaluer les risques potentiels liés à une mauvaise configuration.

L’étude est réalisée sur une application Android volontairement vulnérable afin de simuler des scénarios d’attaques réels dans un 

environnement contrôlé (émulateur Android).

À travers les différentes étapes de ce travail, nous allons :

- Énumérer les applications installées sur l’émulateur
  
- Identifier les composants exposés de l’application cible

- Analyser les permissions et les protections mises en place

- Détecter les failles de sécurité potentielle

- Documenter les résultats sous forme de preuves organisées

## Étape 1 : Configuration de l’environnement

Mettre en place un environnement Android avec Drozer pour l’analyse de la surface d’attaque.

### 1. Lancement de l’émulateur Android

L’émulateur Android a été lancé depuis Android Studio :

<img width="566" height="504" alt="image" src="https://github.com/user-attachments/assets/6feb173a-fb76-44c6-b1e0-a4fc7455e7b1" />

### 2. Installation de Drozer Agent

L’agent Drozer a été installé sur l’émulateur à l’aide de la commande :

<img width="289" height="47" alt="image" src="https://github.com/user-attachments/assets/8dda88f2-5027-4935-b15b-7dd4656ce7f2" />

### 3. Installation de l’application vulnérable

Une application de test volontairement vulnérable a été installée :

<img width="296" height="44" alt="image" src="https://github.com/user-attachments/assets/acb03c32-1ca9-4b63-b81d-d9d3f35723b0" />

### 4. Activation du serveur Drozer

<img width="323" height="439" alt="image" src="https://github.com/user-attachments/assets/e7ccd6d8-7e06-497e-a6f7-09d45adae933" />

<img width="190" height="343" alt="image" src="https://github.com/user-attachments/assets/65912821-3caa-4435-bda1-54e4361f5771" />

### 5. Configuration du port forwarding

Le port forwarding a été configuré pour permettre la communication avec Drozer :

<img width="307" height="31" alt="image" src="https://github.com/user-attachments/assets/d26df198-104b-49a9-8ea8-891b6c85af8e" />

## Étape 2 : Connexion Drozer

### Connexion à la console

<img width="472" height="251" alt="image" src="https://github.com/user-attachments/assets/4fc09b78-df50-404c-bc5e-b1b0e62f3973" />

La connexion avec l’émulateur est établie avec succès.

### Listez les modules disponibles pour vous familiariser avec Drozer :

<img width="824" height="500" alt="image" src="https://github.com/user-attachments/assets/a4adfc6d-3521-43cd-b18f-7177b6d666eb" />

 - Résultat : affichage de plusieurs modules comme :
   
  app.package.list
  
  app.package.attacksurface
  
  app.activity.info
  
  app.provider.info

- Cela confirme que Drozer est correctement installé et opérationnel.

### Informations appareil (test)

<img width="417" height="78" alt="image" src="https://github.com/user-attachments/assets/d51cd4bd-6c25-4578-9796-ef91a8897105" />

### Résultat

- /proc/version: open failed: EACCES (Permission denied)

- Android bloque l’accès aux fichiers système

- Drozer n’a pas les droits root

- Restriction normale sur Android 11

- Les commandes suivantes ne fonctionnent pas dans cette version :

dz> device
dz> run information.device

## Étape 3 : Cartographie des composants Android exposés

1. Identifiez toutes les applications installées sur l'émulateur :

<img width="574" height="488" alt="image" src="https://github.com/user-attachments/assets/b635af8a-cff8-47d6-8f29-1ece7da31f0d" />

<img width="482" height="484" alt="image" src="https://github.com/user-attachments/assets/b5693557-5689-4218-af88-44894457f795" />

2. Obtenez des informations détaillées sur l'application :

<img width="600" height="227" alt="image" src="https://github.com/user-attachments/assets/7841f94a-2d98-497e-9bf0-0d2d7f548105" />

3. Identifiez les activités exportées :

<img width="295" height="237" alt="image" src="https://github.com/user-attachments/assets/c787d955-93fb-4f03-970b-cdc3f4cf18c3" />

4. Identifiez les services exportés :

<img width="365" height="56" alt="image" src="https://github.com/user-attachments/assets/150c2ca8-4368-40e0-8b53-3c3f37219a58" />

5. Identifiez les broadcast receivers exportés :

<img width="373" height="54" alt="image" src="https://github.com/user-attachments/assets/7dc11f62-c788-4b40-9dcb-21fbe3022862" />

6. Identifiez les content providers exportés :

<img width="334" height="121" alt="image" src="https://github.com/user-attachments/assets/c9923683-ba49-405c-8bfb-17e8bdbc3fdb" />

7. Un tableau récapitulatif des composants exposés :

| Type               | Nom               | Exporté | Protection |
| ------------------ | ----------------- | ------- | ---------- |
| Activity           | MainActivity      | Oui     | Aucune     |
| Activity           | APICredsActivity  | Oui     | Aucune     |
| Activity           | APICreds2Activity | Oui     | Aucune     |
| Service            | —                 | Non     | —          |
| Broadcast Receiver | —                 | Non     | —          |
| Content Provider   | notesprovider     | Oui     | Aucune     |

## Étape 4 : Vérification des protections

### Analyse du AndroidManifest.xml :

<img width="429" height="489" alt="image" src="https://github.com/user-attachments/assets/ea34604c-72a0-4952-9695-8e15a786c515" />

<img width="423" height="470" alt="image" src="https://github.com/user-attachments/assets/9a295b56-666f-4a7c-bbe3-9c89715079a8" />

- Application debuggable : TRUE
- allowBackup : TRUE
- Permissions :
  - WRITE_EXTERNAL_STORAGE
  - READ_EXTERNAL_STORAGE
  - INTERNET

### Intent-filters :

<img width="346" height="284" alt="image" src="https://github.com/user-attachments/assets/36e33ace-7ad3-40cc-92e4-79914b21a455" />

- MainActivity → launcher
  
- APICredsActivity → VIEW_CREDS
  
- APICreds2Activity → VIEW_CREDS2

### Content Provider :

<img width="352" height="110" alt="image" src="https://github.com/user-attachments/assets/aff2b0ae-018b-4f19-b0bb-2e411de3334a" />

- NotesProvider est EXPORTÉ (vulnérable)

- Aucune permission définie (Read/Write = NULL)

### Test URI :

<img width="490" height="128" alt="image" src="https://github.com/user-attachments/assets/ae6f7934-14cd-4748-8f2e-62d2a69698bc" />

<img width="337" height="81" alt="image" src="https://github.com/user-attachments/assets/2fc1ebf3-2db5-401f-876f-f11519f6fb29" />

- scanner.provider.finduris a confirmé l’accès direct aux données

- URI accessibles sans authentification

### Conclusion :
L’application présente plusieurs failles de sécurité :
- exposition des composants
- absence de contrôle d’accès
- content provider exploitable
  
## Étape 5 : Analyse des risques

À partir de la cartographie des composants exposés de l’application **jakhar.aseem.diva**, nous avons identifié plusieurs risques de sécurité.

### 1. Activities exportées sans protection

**Composants concernés :**

- MainActivity
  
- APICredsActivity
  
- APICreds2Activity
  
- LogActivity
  
- SQLInjectionActivity
  
- AccessControl1Activity

**Risque :**

- Accès non autorisé à des écrans internes de l’application.

**Scénario d’attaque :**

Un attaquant peut lancer directement une Activity exportée sans passer par l’authentification normale de l’application, ce qui peut permettre de contourner les contrôles d’accès.

**Impact :**

- Fuite d’informations sensibles
  
- Contournement du système d’authentification


### 2. Services exportés

**Observation :**

- Aucun service exporté détecté

**Risque :**

- Faible dans ce cas

### 3. Broadcast Receivers exportés

**Observation :**

- Aucun Broadcast Receiver exporté détecté

**Risque :**

- Aucun risque direct identifié

### 4. Content Providers mal protégés

**Composant concerné :**

- NotesProvider

- Authority : `jakhar.aseem.diva.provider.notesprovider`
  
- Exported : TRUE
  
- Permissions : NULL (aucune protection)

**Risque :**

- Accès non autorisé aux données internes de l’application

**Scénario d’attaque :**

Un attaquant peut interroger directement le Content Provider via les URI accessibles sans authentification afin de lire ou manipuler les données stockées.

**Impact :**

- Fuite de données sensibles
- Modification ou suppression de données

### 5. Permissions insuffisantes

**Permissions utilisées :**

- WRITE_EXTERNAL_STORAGE
  
- READ_EXTERNAL_STORAGE
  
- INTERNET
  
- ACCESS_MEDIA_LOCATION

**Risque :**

- Permissions mal contrôlées ou trop larges

**Scénario d’attaque :**

Une application malveillante peut exploiter ces permissions pour accéder aux fichiers stockés ou intercepter des données.

**Impact :**

- Compromission des données utilisateur
  
- Accès non autorisé au stockage externe

### Conclusion générale

L’application présente plusieurs failles de sécurité importantes :

- Composants exportés sans restriction
- Content Provider non protégé
- Absence de mécanismes d’authentification internes
- Exposition des activités sensibles

Ces vulnérabilités peuvent permettre à un attaquant de :
- contourner la sécurité
- accéder aux données privées
- exécuter des actions non autorisées
  
## Étape 6 : Collecte de preuves

Toutes les preuves de l’analyse Drozer sont organisées dans le dossier suivant :

<img width="380" height="83" alt="image" src="https://github.com/user-attachments/assets/f21ba444-5177-418b-b81f-34539c5183e8" />

