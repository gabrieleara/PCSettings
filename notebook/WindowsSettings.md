# Introduction

# Table of Contents

- [Introduction](#introduction)
- [Table of Contents](#table-of-contents)
- [System Settings](#system-settings)
	- [Sistema](#sistema)
		- [Schermo](#schermo)
		- [Assistente Notifiche](#assistente-notifiche)
		- [Alimentazione e Sospensione](#alimentazione-e-sospensione)
		- [Batteria](#batteria)
		- [Archiviazione](#archiviazione)
		- [Multitasking:](#multitasking)
		- [Esperienze condivise](#esperienze-condivise)
		- [Desktop Remoto](#desktop-remoto)
		- [Informazioni su](#informazioni-su)
	- [Dispositivi](#dispositivi)
		- [Mouse](#mouse)
	- [Telefono](#telefono)
	- [Rete e Internet](#rete-e-internet)
	- [Personalizzazione](#personalizzazione)
		- [Sfondo](#sfondo)
		- [Colori](#colori)
		- [Schermata di blocco](#schermata-di-blocco)
		- [Temi](#temi)
		- [Start](#start)
		- [Barra delle applicazioni](#barra-delle-applicazioni)
	- [App](#app)
		- [App predefinite](#app-predefinite)
	- [Account](#account)
		- [Le tue info](#le-tue-info)
		- [Account e-mail e per le app](#account-e-mail-e-per-le-app)
		- [Sincronizza le impostazioni](#sincronizza-le-impostazioni)
	- [Data e ora](#data-e-ora)
		- [Data e ora](#data-e-ora-1)
		- [Area geografica e lingua](#area-geografica-e-lingua)
	- [Accessibilità](#accessibilità)
		- [Assistente Vocale:](#assistente-vocale)
	- [Cortana](#cortana)
		- [Parla con Cortana](#parla-con-cortana)
		- [Autorizzazzioni e cronologia](#autorizzazzioni-e-cronologia)
	- [Privacy](#privacy)
		- [Generale](#generale)
		- [Riconoscimento vocale, input penna e digitazione](#riconoscimento-vocale-input-penna-e-digitazione)
		- [Feedback e Diagnostica](#feedback-e-diagnostica)
		- [Cronologia attività](#cronologia-attività)
		- [Posizione](#posizione)
	- [Aggiornamento e Sicurezza](#aggiornamento-e-sicurezza)
		- [Windows Update](#windows-update)
- [Drivers](#drivers)
- [TaskBar](#taskbar)
- [App](#app-1)
	- [File](#file)
	- [7-ZIP](#7-zip)
	- [Acrobat Reader DC](#acrobat-reader-dc)
	- [Google Chrome](#google-chrome)
		- [Plugins Configuration](#plugins-configuration)
	- [Google Drive](#google-drive)
	- [Telegram Desktop](#telegram-desktop)
	- [Java JDK 8 (32 e 64 bit)](#java-jdk-8-32-e-64-bit)
	- [VLC Media Player](#vlc-media-player)
	- [NODE.JS](#nodejs)
	- [Nativefier](#nativefier)
	- [WhatsApp](#whatsapp)
	- [ShareLaTeX](#sharelatex)
	- [Calibre](#calibre)
	- [GitHub Desktop](#github-desktop)
	- [Spotify](#spotify)
	- [uTorrent](#utorrent)
	- [XVR Studio 2.0](#xvr-studio-20)
	- [Nvidia GeForce Experience](#nvidia-geforce-experience)
	- [VS Code](#vs-code)
	- [MATLAB](#matlab)
- [Tools](#tools)
	- [Caffeinated](#caffeinated)
	- [Tree Size Free](#tree-size-free)
- [Start Menu](#start-menu)

# System Settings
Following settings are localized in Italian, since that's how I see them on my UI. Missing elements from Windows Settings application are supposed to be left as they are.

## Sistema

### Schermo
- Luce Notturna: **ON**
- Dimensione Testo: **100%**
- Risoluzione: **1366x768**

### Assistente Notifiche
- Disattivato
- Durante la Duplicazione dello schermo: **solo sveglie ON**

### Alimentazione e Sospensione
- Se alimentato a batteria, disattiva dopo: **5 min**
- Se collegato alla rete elettrica, disattiva dopo: **10 min**
- Se alimentato a batteria, sospendi dopo: **15 min**
- Se collegato alla rete elettrica, sospendi dopo: **30 min**

- Impostazioni di Risparmio Energia Aggiuntive
	- Specifica il comportamento quando viene chiuso il coperchio
		- Modifica le impostazioni attualmente non disponibili
			- Ibernazione: **ON**
			- Quando viene premuto il pulsante di alimentazione:
				- A batteria: **arresta il sistema**
				- Rete elettrica: **arresta il sistema**
			- Quando viene chiuso il coperchio
				- A batteria: **iberna**
				- Rete elettrica: **iberna**

### Batteria
- Attiva automaticamente Risparmia batteria sotto al: **30%**
- Riduci la luminosità in risparmio batteria: **ON**

### Archiviazione
- Sensore Memoria: **Disattivato**

### Multitasking:
- Disponi automaticamente finestre trascinandole: **ON**
- Quando ancoro, adattala allo spazio disponibile: **ON**
- Quando ancoro, mostra cosa posso affiancare: **ON**
- Quando ridimensiono una finestra ancorata, ridimensiona quella affiancata: **ON**
- Mostra occasionalmente suggerimenti in Finestra Temporale: **OFF**

- Sulla barra delle applicazioni mostra: **solo nel desktop in uso**
- Con ALT+TAB mostra le finestre: **solo nel desktop in uso**

### Esperienze condivise
- Consenti alle app di altri dispositivi di aprire e inviare messaggi e viceversa: **ON**
- Posso condividere/ricevere da: **solo i miei dispositivi**

### Desktop Remoto
- Abilita Desktop Remoto: **ON**

### Informazioni su
- Nome Dispositivo: **ARA-N53SV**

## Dispositivi
### Mouse
- Opzioni Aggiuntive per il mouse
	- Elan
		- Imposta scorrimento inerziale e naturale

For touchpad configuration some extra effort shall be done. First of all create a file called `touchpad.reg` and fill it with the following code:
```
Windows Registry Editor Version 5.00

[HKEY_CURRENT_USER\Software\Elantech\SmartPad]
"Tap_Enable"=dword:00000001
"Tap_T1_Time"=dword:000000c8
"Tap_One_Finger_Enable"=dword:00000001
"Tap_Two_Finger_Enable"=dword:00000001
"Tap_Two_Finger"=dword:00000002
"Tap_Three_Finger_Enable"=dword:00000001
"Tap_Three_Finger"=dword:00000001
"Palm_Enable"=dword:00000001
"Palm_Slider"=dword:00000004
"Palm_Always_Enable"=dword:00000001
"ThreeFingerMove_Enable"=dword:00000001
"ThreeFingerMoveUp_Enable"=dword:00000001
"ThreeFingerMoveUp_Func"=dword:0000001a
"ThreeFingerMoveUp_Mode"=dword:00000000
"ThreeFingerMoveDown_Enable"=dword:00000001
"ThreeFingerMoveDown_Func"=dword:0000001a
"ThreeFingerMoveDown_Mode"=dword:00000000
"ThreeFingerMoveLeft_Enable"=dword:00000001
"ThreeFingerMoveLeft_Func"=dword:00000003
"ThreeFingerMoveLeft_Mode"=dword:00000000
"ThreeFingerMoveRight_Enable"=dword:00000001
"ThreeFingerMoveRight_Func"=dword:00000004
"ThreeFingerMoveRight_Mode"=dword:00000000
"ThreeFingerMove_UD_ShowItem"=dword:00000001
"ThreeFingerMove_LR_ShowItem"=dword:00000001
"Win8EdgeSwipeLeft_Enable"=dword:00000001
"Win8EdgeSwipeRight_Enable"=dword:00000001
"Win8EdgeSwipeTop_Enable"=dword:00000000
"Win8EdgeSwipeBottom_Enable"=dword:00000000
```

Then open it by double clicking it, so that these registry keywords are set to the desired value. When done, execute `"C:\Program Files\Elantech\ETDAniConf.exe"`, change something (and re-change it back to the previous value, if you don't need to change anything), then press `Apply` so that new Registry Condfiguration is applied.

To achieve the desired control of the desktop environment from the touchpad install **AutoHotKey** from here https://autohotkey.com/download/ahk-install.exe and create a new file called `gestures.ahk`, with following content:
```
Browser_Back::SendInput {LCtrl down}{LWin down}{Right}{LCtrl up}{LWin up}
Browser_Forward::SendInput {LCtrl down}{LWin down}{Left}{LCtrl up}{LWin up}
#F::Send {LWin down}{Tab}{LWin up}
#C::SendInput {LCtrl down}{LWin down}{Right}{LCtrl up}{LWin up}
#^BS::SendInput {LCtrl down}{LWin down}{Left}{LCtrl up}{LWin up}
```

Finally, create a symbolic link to that file in

```
"C:\Users\gabriele\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup"
```

Once the pc is reeboted, it should all be configured.

## Telefono
TODO

## Rete e Internet
TODO: set custom DNS

## Personalizzazione
### Sfondo
- Presentazione: **tema sincronizzato**
- Cambia immagine ogni: **1 ora**
- Riproduzione casuale: **ON**
- Presentazione con batteria: **OFF**
- Posizione: **Riempi**

### Colori
TODO

### Schermata di blocco
TODO

### Temi
Tema sincronizzato

### Start
- Più riquadri: **OFF**
- Elenco delle app: **ON**
- App aggiunte di recente: **OFF**
- App più usate: **OFF**
- Modalità schermo intero: **OFF**
- Elementi aperti di recente: **ON**

### Barra delle applicazioni
- Blocca la barra: **ON**
- Nascondi automaticamente in modalità desktop: **OFF**
- Nascondi automaticamente in modalità tablet: **OFF** 
- Usa pulsanti piccoli: **OFF**
- Usa Aero Peek: **ON**
- Sostituisci il prompt con la power sheell: **OFF/ON** (non ha importanza)
- Mostra le notifiche: **ON**
- Posizione: **in basso**
- Combina: **sempre, mostra solo icone**
- Più Schermi: **OFF**
- Contatti: **OFF**

## App

### App predefinite
- Lettore Musicale: **VLC media player**
- Lettore Video: **VLC media player**
- Browser Web: **Google Chrome**

## Account
### Le tue info
Accedi all'account Microsoft

### Account e-mail e per le app
TODO: add mother email

### Sincronizza le impostazioni
Tutto: **ON**


## Data e ora
### Data e ora
- Imposta data/ora automaticamente: ON
- Imposta fuso orario automaticamente: ON

### Area geografica e lingua
Install `Tastiera Italiana per Programmatori`, which can be found in the following [folder](../generic/italian-programmer-keyboard/it-prog).
TODO: add link

## Accessibilità
### Assistente Vocale:
- Consenti l'uso del tasto di scelta rapida per avviare l'assistente vocale: **OFF**

## Cortana
### Parla con Cortana
- Ehi Cortana: **OFF**
- Scelta rapida: **OFF**
- Schermata di blocco: **OFF**
### Autorizzazzioni e cronologia
- Ricerca Sicura: **moderata**
- Cronologia: **ON**

## Privacy
### Generale
- ID Annunci: **OFF**
- Permetti a siti web di accedere all'elenco delle lingue: **ON**
- Consenti a Windows di tenere traccia dell'avvio delle app: **ON**

### Riconoscimento vocale, input penna e digitazione
Disattivato

### Feedback e Diagnostica
- Dati diagnostica: **base** 
- Migliora penna e digitazione: **OFF**
- Esperienze su misura: **OFF**
- Visualizzatore dati diagnostica: **OFF**

### Cronologia attività
- Consenti a windows di raccogliere le mie attività su questo PC: **ON**
- Consenti a windows di sincronizzare sul cloud: **OFF**

### Posizione
- Disattivato

## Aggiornamento e Sicurezza
### Windows Update
- Opzioni Avanzate
	- Scarica aggiornamenti per altri prodotti Microsoft: **ON**
	- Download automatico anche su connessioni a consumo: **OFF**
	- Più promemoria al riavvio: **OFF**
	- Sospendi aggiornamenti: **OFF**
	- Quando installare: **canale semestrale (mirato)**

# Drivers

~~Almost~~ All drivers are available here: https://ivanrf.com/en/latest-asus-drivers-for-windows-10/

Required drivers are the following ones:
- ATKPackage
- Card Reader (?) TODO
- Fresco Logic USB 3.0 Host Controller

# TaskBar
- Elenco delle app
	1. File
	2. Chrome
	3. Telegram
	4. WhatsApp
	5. VS Code
	6. VLC
	7. Spotify
	8. MATLAB
	9. ShareLaTeX
	10. Calibre
	11. uTorrent
	12. XVR

- Tasto destro
	- Cortana: **nascosta**
	- Mostra pulsante visualizza attività: **ON**

# App
## File
- Visualizza Estensioni: **ON**
- Visualizza File Nascosti: **OFF**
- Opzioni
	- Apri Esplora File per: **Questo PC**
	- Mostra file usati di recente: **OFF**
	- Mostra cartelle create di recente: **OFF**

## 7-ZIP
- Download Link: https://www.7-zip.org/

## Acrobat Reader DC
- Download Link: https://get.adobe.com/it/reader/
- Set Acrobat as default PDF reader
- Log into Document Cloud 

## Google Chrome
- Download Link: https://www.google.it/chrome/index.html
- Set it as default browser
- Log in to Google account
- Status bar -> right click -> Lascia Google Chrome in background: **OFF**

### Plugins Configuration
- The Great Suspender:
	- Automatically Suspend: **30 minutes**
	- Keyboard Shortcuts > Remap Keys > Delete any mapping
	- Whitelist:
```
facebook.com google.com TODO
```
	
- Feedly:
	- Open `chrome-extension://hipbfijinpcgfogaopmgehiegacbhmob/gc-options.htm`
	- Show Feedly Mini Icon: **OFF**
- STANDS
	- Just enable it

## Google Drive
- Download Link: https://www.google.com/drive/download/
- Log in using chrome
- Disable PC folders synchronization on cloud
- When done with installation, open preferences:
	- Dispositivi USB e schede SD > Backup di Fotocamera: **OFF**

## Telegram Desktop
- Download Link: https://telegram.org/dl/desktop/win
- Log in
- Settings
	- Set Italian as language
	- Use Windows notifications
	- Launch Telegram at Windows startup
	- Launch as icon in system tray
	- Add Telegram to "send to" menu
	- Change background

## Java JDK 8 (32 e 64 bit)
- Download Link: http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html

## VLC Media Player
- Download Link: https://www.videolan.org/vlc/

## NODE.JS
- Download Link: https://nodejs.org/en/download/

## Nativefier
- Open command prompt
- Run: `npm install nativefier -g `
- Create folder:	`"C:\Tools\WebApps"`
- Crea folder:  `"C:\Users\gabriele\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\WebApps"`

## WhatsApp
- Open command prompt
- Run: `cd "C:/Tools/WebApps/"`
- Run: `nativefier -p win32 -a x64 -n WhatsApp 	-tray 	-single-instance web.whatsapp.com`
- Open new folder using File Explorer
- Create a Desktop shortcut
- Copy the new shortcut in `"C:\Users\gabriele\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\WebApps"`

## ShareLaTeX
- Open command prompt
- Run: `cd "C:/Tools/WebApps/"`
- Run: `nativefier -p win32 -a x64 -n ShareLaTeX www.sharelatex.com`
- Open new folder using File Explorer
- Create a Desktop shortcut
- Copy the new shortcut in `"C:\Users\gabriele\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\WebApps"`

## Calibre
- Download Link: https://calibre-ebook.com/dist/win64
- Change library folder to following path: `"C:\Users\gabriele\OneDrive\Libri\Narrativa"`
- Set Kindle Paperwhite as default reader:
	- Amazon
	- Kindle PaperWhite
	- To configure Kindle e-mail, checkout this link: https://www.amazon.it/hz/mycd/myx#/home/devices
- TODO: Other Settings

## GitHub Desktop
- Download Link: https://central.github.com/deployments/desktop/desktop/latest/win32
- Log in

## Spotify
- Download Link: https://download.scdn.co/SpotifySetup.exe
- Log in using Facebook
- Modifica > Preferenze
	- Streaming ad alta qualità: **ON**
	- Normalizza volume: **ON**
	- Livello: **Normale**
	- Mostra brani di:
		- Download: **OFF**
		- Libreria Musicale: **ON**
- Mostra Impostazioni Avanzate
	- Autoplay: **ON**
	- Dissolvenza: **OFF**
	- Apri Spotify Automaticamente: **NO**
	- Pulsante chiudi lo porta nella barra delle applicazioni: **ON**
	- Consenti di aprire Spotify dal web: **ON**

## uTorrent
- Download Link: https://www.utorrent.com/intl/it/downloads/complete/track/stable/os/win
- Opzioni > Impostazioni
	- Avvia uTorrent all'avvio di Windows: **OFF**
	- Avvia ridotto a icona: **OFF**
	- Avanzate: change these key-value pairs:
		- `gui.show_plus_upsell						false`
		- `offers.sponsored_torrent_offer_enabled	false`
		- `offers.left_rail_offer_enabled			false`
		- `gui.show_notorrents_node					false`
		- `offers.content_offer_autoexec			false`
		- `bt.enable_pulse							false`

## XVR Studio 2.0
- Download Link: https://sourceforge.net/projects/xvrstudio/files/latest/download?_test=goal
- OpenAL
	- Download Link: https://www.openal.org/downloads/oalinst.zip

## Nvidia GeForce Experience
- Download Link: https://www.nvidia.it/geforce/geforce-experience/

## VS Code
- Download Link: https://code.visualstudio.com/docs/?dv=win64
- Download Git Link: https://git-scm.com/download/win
- TODO: Configuration


## MATLAB
- Download Link: https://www.mathworks.com/downloads/
- Updates Download Link: https://it.mathworks.com/downloads/web_downloads/show_updates?release=R2018a
- TODO: Configuration


# Tools
- Create the following folders:
```
"C:\Tools"
"C:\Users\gabriele\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Tools"
```

## Caffeinated
- Download Link: https://github.com/downloads/dmnd/Caffeinated/Caffeinated-1.0.zip
- Extract it inside `"C:\Tools\"`
- Create a Desktop Shortcut
- Move the new shortcut to `"C:\Users\gabriele\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Tools"`
- Launch it (download .NET if necessary)
- Configuration
	- Automatically launch at Windows startup: **ON**
	- Activate upon launch: **OFF**
	- Show this message upon launch: **OFF**
	- Default Duration: **Indefinitely**

## Tree Size Free
- Download Link: https://www.jam-software.de/customers/downloadTrial.php?article_no=80&language=EN
- Extract it inside `"C:\Tools\"`
- Create a Desktop Shortcut
- Move the new shortcut to `"C:\Users\gabriele\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Tools"`


# Start Menu
TODO

