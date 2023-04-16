# Utilisation d'un service principal dans un script powershell

* Création d'un certificat auto-signé
* Convertir le certificat en base64
* création d'un application Azure Active Directory
* intégré la connexion au script powershell

## Création d'un certificat auto-signé

l'avantage de cette solution c'est que toutes les machines sur lesquelles ce script devra tourner auront besoin de ce certificat

Si le script tombe entre de mauvaises mains, sans le certificat adequate il ne fonctionnera pas

Ouvrir un prompt powershell en administrateur pour créer le certificat (ici auto-signé pour limiter les couts)

```powershell
$pwd = "MaPassPhraseLaPlusComplexeDuMonde1234"
$thumb = (New-SelfSignedCertificate -DnsName "script.mydomain.com" -CertStoreLocation "cert:\LocalMachine\My"  -KeyExportPolicy Exportable -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider" -NotAfter (Get-Date).AddMonths(24)).Thumbprint
$pwd = ConvertTo-SecureString -String $pwd -Force -AsPlainText
Export-PfxCertificate -cert "cert:\localmachine\my\$thumb" -FilePath c:\temp\MonCertificat.pfx -Password $pwd
```

## Convertir le certificat en base64

```powershell
$cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate("C:\temp\MonCertificat.pfx", "MaPassPhraseLaPlusComplexeDuMonde1234")
$keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData()) | Out-File c:\temp\examplecert_base64.crt
```
## Création de l'application Azure Active Directory

* Se connecter au portail Azure : https://portal.azure.com/
* Rechercher ```App registration```
![App Registration](../Images/20221207%20-%20Utiliser%20Authentification%20moderne%20pour%20les%20scripts%20PS/App%20registration.png)
* Cliquer sur ```New Registration```
    * Donner un nom explicite pour l'application
    * Dans ```Redirect URL``` choisir ```Web```
* Cliquer sur ```Register```
* Dans le Menu ```Manage``` cliquer sur ```Api Permissions```
![API Permissions](../Images/20221207%20-%20Utiliser%20Authentification%20moderne%20pour%20les%20scripts%20PS/API%20permissions.png)

A partir de ce menu on peut donner toute les permissions necessaire pour l'utilisation de cette application

* Dans le menu ```Manage``` cliquer sur ```Cetificates & Secrets``` puis ```Certificate```
* Cliquer sur ```Upload Certificate``` et selectionner le fichier de certificat base 64 précédement créé
![Upload Certificate](../Images/20221207%20-%20Utiliser%20Authentification%20moderne%20pour%20les%20scripts%20PS/Upload%20Certificate.png)
* Copier le ```Thumprint``` nous en aurons besoin plus tard


##Intégrer la connexion au script powershell

Sur la machine qui va faire tourner le script powershell necessitant l'utilsation de cette application

* Récupérer le certificat précédement créé (en pfx) et le coller sur la machine
* double cliquer sur le certificat pour l'installer (machine ou utilisateur). Dans mon cas je l'enregistre sur Ordinateur pour que tous le monde puisse l'utiliser
* récupérer les propriétés de connexion pour le script 
    * ```$TenantID``` : l'ID de votre Tenant (Dans AzureAD dans le menu ```Manage``` => ```Properties``` => TenantID)
    * ```$ApplicationID``` : Sur la page d'enregistrement de votre application
    * ```$Thumbprint```:  le thumbprint précédement récupérer lors de l'import du certificat base64

    Pour se connecter il faut ajouter a votre script powershell 

```Powershell
$tenantID = "<Your Tenant ID>"
$applicationID = "<Your Application ID>"
$thumbprint = "<Your Certificate Thumbprint>"
 
Connect-MgGraph -ClientID $applicationID -TenantId $tenantID -CertificateThumbprint $thumbprint
 
# Ger all Azure AD Users
Get-MgUser
```