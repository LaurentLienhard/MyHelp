# Tester un script dans le contexte d'un compte de service managé

* Installation PSTools
* Lancement d'un contexte MSA
* Execution du script

## Installation PSTools

Pour pouvoir lancer un test dans un contexte d'un compte de service managé, je vais utiliser ```PSExec```. 

Pour cela il va falloir l'installer. Le plus simple est de télécharger la dernière version de PSTools [ici](https://download.sysinternals.com/files/PSTools.zip), de dézipper le dossier et de le déposer sur le serveur hébergeant le comtpe de service managé

## Lancement du contexte MSA

Se rendre sur le serveur hébergeant le compte de service managé et lancer la commande
```text
./PsExec64.exe -i -u DOMAINE\MsaAccount$ -p ~ powershell.exe
```

Le ```~``` signifie au système qu'il doit chercher le mot de passe dans l'AD

## Execution d'un script

A partir de ce moment votre session PowerShell tourne dans le contexte du compte de service managé et vous pouvez donc tester si votre script tourne correctement aec ce compte utilisateur

