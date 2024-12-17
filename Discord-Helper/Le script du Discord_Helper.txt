@echo off
setlocal enabledelayedexpansion

:: Configuration couleur (fond noir, texte bleu clair)
color 09

:title
cls
echo ----------------------------------------------
echo         BIENVENUE DANS LE DISCORD HELPER
echo ----------------------------------------------
echo Developpe par Martim avec l'aide de Vidya !
pause

:menu
cls
echo ----------------------------------------------
echo          MENU PRINCIPAL DU DISCORD HELPER
echo ----------------------------------------------
echo 1. Webhook - Envoyer un message
echo 2. Ouvrir une invitation de serveur
echo 3. Ressources utiles
echo 4. Proposer vos idees
echo 5. Quitter
echo ----------------------------------------------
set /p choice=Faites votre choix (1/2/3/4/5) :

if "%choice%"=="1" goto webhook
if "%choice%"=="2" goto serveur
if "%choice%"=="3" goto ressources
if "%choice%"=="4" goto propositions
if "%choice%"=="5" goto end

:: Si choix invalide
echo Choix invalide. Veuillez reessayer.
pause
goto menu

:webhook
cls
echo ----------------------------------------------
echo      ENVOYER UN MESSAGE VIA UN WEBHOOK
echo ----------------------------------------------
set /p webhookUrl=Entrez l'URL du webhook : 
if not defined webhookUrl (
    echo L'URL du webhook est manquante. Veuillez reessayer.
    pause
    goto webhook
)
set /p message=Entrez votre message : 
if not defined message (
    echo Le message est manquant. Veuillez reessayer.
    pause
    goto webhook
)
:: Envoi du message via PowerShell
powershell -Command "Invoke-RestMethod -Uri '!webhookUrl!' -Method Post -Body (@{content='!message!'} | ConvertTo-Json) -ContentType 'application/json'"
echo Message envoye avec succes !
pause
goto menu

:serveur
cls
echo ----------------------------------------------
echo        OUVRIR UNE INVITATION DE SERVEUR
echo ----------------------------------------------
set /p inviteLink=Entrez le lien d'invitation du serveur : 
if not defined inviteLink (
    echo Le lien est manquant. Veuillez reessayer.
    pause
    goto serveur
)
start "" "!inviteLink!"
echo Serveur ouvert dans votre navigateur !
pause
goto menu

:ressources
cls
echo ----------------------------------------------
echo           RESSOURCES UTILES DISCORD
echo ----------------------------------------------
echo 1. Installer Vencord
echo 2. Bots populaires (via navigateur)
echo 3. Guide pour debutants
echo 4. Retour au menu principal
echo ----------------------------------------------
set /p ressourcesChoice=Faites votre choix (1/2/3/4) :

if "%ressourcesChoice%"=="1" (
    echo Ouverture de la page Vencord...
    start "" "https://vencord.dev/download/"
    pause
    goto ressources
)
if "%ressourcesChoice%"=="2" (
    echo Ouverture de la page des bots Discord populaires...
    start "" "https://top.gg"
    pause
    goto ressources
)
if "%ressourcesChoice%"=="3" (
    echo Redirection vers un guide pour debutants...
    start "" "https://support.discord.com/hc/fr/articles/360045138571-D%C3%A9buter-sur-Discord"
    pause
    goto ressources
)
if "%ressourcesChoice%"=="4" goto menu

:: Si choix invalide
echo Choix invalide. Retour au menu ressources.
pause
goto ressources

:propositions
cls
echo ----------------------------------------------
echo      PROPOSEZ VOS IDEES POUR AMELIORER
echo            LE DISCORD HELPER !
echo ----------------------------------------------
echo Vous avez une idee pour ameliorer Discord_Helper ?
echo Envoyez-la a l'adresse suivante :
echo martim.pro.go@gmail.com
echo Ou contactez-moi sur Discord : @martim.brg
echo ----------------------------------------------
echo Si vous souhaitez m'envoyer un e-mail, appuyez sur 'E'.
echo Si vous souhaitez m'envoyer un message Discord, appuyez sur 'D'.
echo ----------------------------------------------
set /p contactChoice=Faites votre choix (E/D) : 

if "%contactChoice%"=="E" (
    echo Ouverture de votre client mail...
    start "" "mailto:martim.pro.go@gmail.com?subject=Idees%2 pour%2 ameliorer%2 le %2Discord_Helper"
    pause
    goto menu
)

if "%contactChoice%"=="D" (
    echo Vous pouvez me contacter sur Discord : @martim.brg
    call :open_discord
    pause
    goto menu
)

:: Si choix invalide
echo Choix invalide. Retour au menu principal.
pause
goto menu

:open_discord
:: Tenter d'ouvrir Discord selon le client (Web ou local)
cls
echo Ouverture de Discord...
:: Si Discord est local
where discord.exe >nul 2>nul
if %errorlevel%==0 (
    echo Discord local detecte, ouverture vers la liste d'amis...
    start discord://friends
) else (
    echo Discord web non detecte. Ouverture de la version web...
    start "" "https://discord.com/channels/@me"
)
goto menu

:end
cls
echo Merci d'avoir utilise le Discord Helper. A bientot !
pause
exit
