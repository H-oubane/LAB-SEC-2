# Lab Rooting Android - Walkthrough

## Objectif
Comprendre ce que le rooting change sur Android, comment vérifier l'état du système, et comment remettre l'environnement à zéro.

## Environnement
- PC hote : Windows
- Emulateur : Genymotion Android 11 API 30
- Outils : ADB (Android Debug Bridge)

---

## Etape 1 : Vérifier la connexion à l'AVD

Commande :
```bash
adb devices
Resultat :

text
192.168.56.102:5555     device
```

<img width="1600" height="899" alt="image" src="https://github.com/user-attachments/assets/a80027d4-b52c-4f99-ac52-9545fd6ac93e" />

## Etape 2 : Activer le mode root
Commande :
```
bash
adb root
Resultat :

text
adbd is already running as root
``` 
<img width="667" height="76" alt="image" src="https://github.com/user-attachments/assets/d4ed29e0-7ad5-45fa-bb78-6b3b64ab7efd" />

## Etape 3 : Remonter la partition système en lecture/écriture
Commande :
```
bash
adb remount
Resultat :

text
remount succeeded
```
<img width="616" height="122" alt="image" src="https://github.com/user-attachments/assets/a2770bf8-e921-4cd4-b66c-676083bf35ec" />

## Etape 4 : Vérifier les privilèges root
Commande :
```
bash
adb shell id
Resultat :

text
uid=0(root) gid=0(root) groups=0(root),...
Interpretation : uid=0(root) confirme les privilèges administrateur.
```
<img width="1600" height="116" alt="image" src="https://github.com/user-attachments/assets/f9c825fd-bc77-4615-bde2-e40470c7b10c" />

## Etape 5 : Tester la commande su
Commande :
```
bash
adb shell "su -c id"
Resultat :

text
uid=0(root) gid=0(root) groups=0(root)...
```

<img width="842" height="178" alt="image" src="https://github.com/user-attachments/assets/15f13d72-b300-427d-9b56-507736523a4f" />

## Etape 6 : Vérifier la version Android
Commande :
```
bash
adb shell getprop ro.build.version.release
Resultat :

text
11
```
<img width="845" height="107" alt="image" src="https://github.com/user-attachments/assets/34e76208-0ffd-4aad-aff0-7039f633cc53" />


## Etape 7 : Capturer les logs système
Commande :
```
bash
adb logcat -d > logcat_root_check.txt
Fichier cree contenant les logs systeme.
```
<img width="876" height="142" alt="image" src="https://github.com/user-attachments/assets/9ae7d707-b93e-4a44-822b-ede5ae98d2a6" />

<img width="1600" height="868" alt="image" src="https://github.com/user-attachments/assets/9d526e4b-16d6-4c29-8c4e-d8e7d1088557" />


## Etape 8 : Télécharger et installer une application de test
Application choisie : F-Droid (magasin d'applications open source)

Téléchargement :
```
bash
adb install F-Droid.apk
Resultat :

text
Success
```
<img width="866" height="138" alt="image" src="https://github.com/user-attachments/assets/479b831d-b9d5-464a-b86c-7421dc47acd9" />

## Etape 9 : Trouver l'activité principale de l'application
Commande :
```
bash
adb shell cmd package resolve-activity --brief org.fdroid.fdroid
Resultat :

text
org.fdroid.fdroid/.views.main.MainActivity
```
<img width="1103" height="93" alt="image" src="https://github.com/user-attachments/assets/6c56a352-0f4e-4130-aec7-1643429d7495" />


## Etape 10 : Lancer l'application
Commande :
```
bash
adb shell am start -n org.fdroid.fdroid/.views.main.MainActivity
Resultat : Application lancee sur l'emulateur.
```
<img width="1600" height="836" alt="image" src="https://github.com/user-attachments/assets/6c8904b3-1f32-4aef-b5c2-75cb515b92d3" />

<img width="498" height="1012" alt="image" src="https://github.com/user-attachments/assets/f8895f7d-33bf-46fc-9bc4-0153bba78458" />


## Etape 11 : Vérifier l'accès aux données de l'application avec privilèges root
Commande :
```
bash
adb shell ls -la /data/data/org.fdroid.fdroid/
Resultat : Acces au dossier de l'application (accessible uniquement avec root).
```
<img width="1067" height="307" alt="image" src="https://github.com/user-attachments/assets/5f5df0d0-20d4-4257-a218-196b69700b2d" />

## Etape 12 : Résumé des commandes rooting
Commande	Objectif	Resultat
adb devices	Verifier connexion	device
adb root	Activer mode root	already running as root
adb remount	Remonter /system	succeeded
adb shell id	Verifier uid	uid=0(root)
adb shell su -c id	Tester su	uid=0(root)


## Etape 13 : Fiche périmètre

FICHE PÉRIMÈTRE - Lab Rooting Android

Application : F-Droid
Version : dernière version
Support : Genymotion AVD Android 11 API 30
Objectif : Comprendre le rooting et ses impacts sur la sécurité Android
Données : fictives uniquement
Réseau : Host-Only
Date : 26/04/2026
Analyste : analyste

## Etape 14 : Résumé Android Security

La sécurité Android repose sur trois piliers :
1. Sandboxing : chaque application est isolée des autres
2. Modèle de permissions : l'utilisateur contrôle l'accès aux données
3. Intégrité du système : vérification au démarrage (Verified Boot)
   
## Etape 15 : Résumé Verified Boot

Verified Boot est un mécanisme qui vérifie l'intégrité du système au démarrage.
Chaque composant vérifie le suivant (chain of trust).
Couleurs : Green (OK), Yellow/Orange (modifié), Red (compromis).

## Etape 16 : Résumé AVB (Android Verified Boot)

AVB est la version 2.0 de Verified Boot.
Ajoute la protection anti-rollback (empêche d'installer une ancienne version vulnerable).
Vérifie l'intégrité des partitions système, vendor, boot.

## Etape 17 : Définition du rooting

Root = privilèges super-utilisateur sur Android.
Le rooting modifie les protections et la confiance du système.
Utile en laboratoire pour observer des comportements normalement inaccessibles.
Risque élevé, nécessite isolement, traçabilité et reset complet.

## Etape 18 : Intérêt du rooting en laboratoire

En laboratoire, un environnement privilégié permet de :
- Observer des artefacts système normalement inaccessibles
- Analyser le comportement des applications à bas niveau
- Tester la robustesse du stockage face à un attaquant privilégié
- Valider les mécanismes de détection de root
  
## Etape 19 : Matrice des risques (8 risques)

1. Intégrité non garantie : conclusions potentiellement fausses
2. Surface d'attaque accrue : exposition à des menaces externes
3. Données sensibles exposées : fuite potentielle
4. Instabilité système : tests non reproductibles
5. Mélange comptes perso/test : fuite d'informations personnelles
6. Mauvais nettoyage : persistance de données sensibles
7. Réseau non isolé : effets involontaires
8. Traçabilité insuffisante : impossibilité d'auditer
   
## Etape 20 : Mesures défensives (8 mesures)

1. Réseau isolé : empêche les communications non contrôlées
2. Données fictives uniquement : aucun risque de fuite réelle
3. AVD/device dédié : exclusivement pour les tests
4. Snapshots ou wipe : aucune trace laissée
5. Journal de configuration détaillé : reproductibilité
6. Aucun compte personnel : pas de mélange de données
7. Contrôle strict des APK : limite les risques
8. Horodatage et captures : traçabilité complète
   
## Etape 21 : OWASP MASVS (2 exigences)

MASVS-STORAGE-1 : Les donnees sensibles doivent etre stockees avec chiffrement.
MASVS-NETWORK-1 : Les communications doivent utiliser TLS avec verification des certificats.

## Etape 22 : OWASP MASTG (2 idées de tests)

MASTG-TEST-1 : Verifier les SharedPreferences dans /data/data/[package]/shared_prefs/
MASTG-TEST-2 : Analyser les logs Android pour detecter des fuites d'informations

## Etape 23 : Fiche environnement complétée

FICHE ENVIRONNEMENT - Lab Rooting

Date : 26/04/2026
Auteur : analyste
Support : Genymotion AVD
Version Android : 11 API 30
Application testée : F-Droid (org.fdroid.fdroid)
Version : dernière version
3 Scénarios : lancer application, naviguer dans les paramètres, quitter
Rooting effectué : OUI
Etat Verified Boot : non disponible (emulateur)
Etat selinux : permissive
Preuves : logs et captures

## Etape 24 : Reset de l'environnement (fin de session)
Commande de wipe data pour l'AVD :
```
bash
adb emu avd stop
adb emu avd wipe-data
```

-Alternative via l'interface Genymotion : Device Manager -> Wipe Data

-Vérification du reset : l'émulateur redémarre avec l'assistant de configuration initial.

## Etape 25 : Checklist finale
-DEBUT :

Périmètre écrit

AVD neuf démarré

Application F-Droid installée

3 scénarios notés

Version Android notée (11)

-FIN :

Aucun compte personnel utilisé

Commandes documentées

Logs capturés

Reset effectué (à faire en fin de séance)

## Conclusion
Le rooting de l'AVD Genymotion a été réussi. Les commandes ADB ont permis d'obtenir les privilèges root (uid=0). Le système est en mode permissif. L'application F-Droid a été téléchargée, installée et lancée avec succès.

Ce lab démontre :

-Comment obtenir les privilèges root sur un AVD

-Quelles commandes permettent de vérifier l'état du système

-L'importance de la traçabilité et du reset en fin de session

-Les risques et mesures défensives associés au rooting

## Commandes utiles (recapitulatif)
```
bash
adb devices                                      # Verifier connexion
adb root                                         # Activer mode root
adb remount                                      # Remonter /system
adb shell id                                     # Verifier uid
adb shell getprop ro.build.version.release       # Version Android
adb shell "su -c id"                             # Tester su
adb logcat -d > logs.txt                         # Capturer les logs
adb install F-Droid.apk                          # Installer une APK
adb shell cmd package resolve-activity --brief org.fdroid.fdroid  # Trouver activite
adb shell am start -n org.fdroid.fdroid/.views.main.MainActivity  # Lancer app
adb uninstall org.fdroid.fdroid                  # Desinstaller une app
adb emu avd wipe-data                            # Wipe de l'AVD
```


## Liens utiles
-F-Droid : https://f-droid.org/

-OWASP MASVS : https://mas.owasp.org/MASVS/

-OWASP MASTG : https://mas.owasp.org/MASTG/

-Android Security : https://source.android.com/docs/security

## Auteur
**H-oubane**

End of walkthrough

text
