# Lenovo TB-X103F Home Assistant Dashboard Setup

**La repository non é ancora completa di tutti i passaggi, nei prossimi giorni verrá ultimata**

Questa repository fornisce una guida dettagliata per trasformare un tablet Lenovo TB-X103F in una dashboard dedicata per Home Assistant.

## Indice

1. [Requisiti](#requisiti)
   - [Hardware Necessario](#hardware-necessario)
   - [Software Necessario](#software-necessario)
2. [Istruzioni di Configurazione](#istruzioni-di-configurazione)
   1. [Sblocco del Fastboot](#1-sblocco-del-fastboot)
   2. [Installazione della Recovery TWRP](#2-installazione-della-recovery-twrp)
   3. [Installazione della Custom ROM](#3-installazione-della-lineageos)
   4. [Preparazione del Tablet](#4-preparazione-del-tablet)
   5. [Configurazione della Dashboard](#5-configurazione-della-dashboard)
   6. [Montaggio e Alimentazione](#6-montaggio-e-alimentazione)
   7. [Manutenzione e Aggiornamenti](#7-manutenzione-e-aggiornamenti)
   8. [Kiosk Mode](#8-modalitá-kiosco)
4. [Risorse Utili](#risorse-utili)

## Requisiti

### Hardware Necessario
- **Tablet Lenovo TB-X103F**
- **Scheda microSD** (anche da **2GB**): per installare la custom ROM
- **PC** con **Windows**, **macOS** o **Linux**: per la preparazione del software e file necessari
- **Caricabatterie** e **cavo USB**: per mantenere il tablet alimentato costantemente e collegarlo al pc

### Software Necessario
- **Home Assistant**: installato e configurato su un server o Raspberry Pi
- **Home Assistant Companion App**: disponibile su [Google Play Store](https://play.google.com/store/apps/details?id=io.homeassistant.companion.android&pcampaignid=web_share), [Apple App Store](https://apps.apple.com/it/app/home-assistant/id1099568401), [F-Droid](https://f-droid.org/it/packages/io.homeassistant.companion.android.minimal/) e [SIDEQUEST](https://sidequestvr.com/app/6427/home-assistant)
- **Fully Kiosk Browser o WallPanel (opzionali)**: per trasformare il tablet in modalità Kiosk
- **ADB (Android Debug Bridge)** e **Fastbook**: [Link](https://developer.android.com/tools/releases/platform-tools?hl=it)
- **Custom ROM LineageOS Unofficial**: [Link](https://www.androidfilehost.com/?fid=4349826312261644319)
- **Custom Recovery TWRP**: [Link](https://www.mediafire.com/file/yw8og5s4n3b9bea/331Lenovo_Tab_10_TB-X103F.rar/file)
- **Gaps (opzionali): [Link](https://opengapps.org/)

## Istruzioni di Configurazione

### 1. Sblocco del Fastboot

1. **Abilita le Opzioni Sviluppatore**:
    - Vai su **Impostazioni** > **Info sul tablet**.
    - Tocca **Numero Build** più volte fino a quando non appare il messaggio "Ora sei uno sviluppatore".

2. **Abilita il Debug USB**:
    - Vai su **Impostazioni** > **Opzioni sviluppatore**.
    - Abilita **Debug USB** e **Sblocco OEM**.

3. **Collegamento del Tablet al PC**:
    - Collega il tablet al PC tramite cavo USB.
    - Assicurati che i driver USB per Lenovo siano installati sul tuo PC.

4. **Riavvio in modalità Fastboot**:
    - Apri un terminale (cmd su Windows, Terminal su macOS/Linux).
    - Esegui il comando per riavviare il tablet in modalità fastboot:
      ```bash
      adb reboot bootloader
      ```
    - Il tablet si riavvierà in modalità fastboot.

5. **Sblocco del Bootloader**:
    - Una volta in modalità fastboot, sblocca il bootloader con il comando:
      ```bash
      fastboot oem unlock
      ```
    - Conferma l'operazione sul tablet (questo cancellerà tutti i dati sul dispositivo).

6. **Riavvio del Tablet**:
    - Riavvia il tablet con:
      ```bash
      fastboot reboot
      ```
    - Il tablet si riavvierà con il bootloader sbloccato.

### 2. Installazione della Recovery TWRP

1. **Download della Recovery TWRP**:
    - Scarica il file immagine della TWRP specifico per Lenovo TB-X103F da [TWRP Official Website](https://twrp.me/Devices/).
    - Salva il file `.img` in una cartella facile da trovare sul tuo PC.

2. **Collegamento del Tablet al PC**:
    - Collega nuovamente il tablet al PC in modalità fastboot utilizzando il comando:
      ```bash
      adb reboot bootloader
      ```
    - Verifica che il dispositivo sia riconosciuto dal PC con:
      ```bash
      fastboot devices
      ```
    - Dovresti vedere il numero di serie del tuo dispositivo nella lista.

3. **Installazione della Recovery TWRP**:
    - Esegui il comando per flashare la TWRP sul tablet:
      ```bash
      fastboot flash recovery twrp.img
      ```
    - Assicurati di sostituire `twrp.img` con il nome esatto del file scaricato.

4. **Riavvio in modalità Recovery**:
    - Dopo aver installato TWRP, riavvia il tablet direttamente in recovery con:
      ```bash
      fastboot reboot recovery
      ```
    - Il tablet dovrebbe avviarsi in TWRP Recovery.

5. **Verifica dell'Installazione**:
    - Una volta in TWRP, verifica che la recovery sia funzionante e fai un backup completo del sistema.

### 3. Installazione della LineageOS

1. **Download della ROM LineageOS**:
    - Scarica l'ultima versione di LineageOS compatibile con Lenovo TB-X103F dal [sito ufficiale di LineageOS](https://lineageos.org/).
    - Scarica anche le **GApps** (Google Apps) se desideri avere i servizi Google sul tablet.
    - Copia entrambi i file `.zip` (ROM LineageOS e GApps) nella scheda microSD.

2. **Inserimento della Scheda microSD**:
    - Spegni il tablet e inserisci la scheda microSD contenente i file scaricati nello slot dedicato.

3. **Avvio in TWRP Recovery**:
    - Accendi il tablet tenendo premuto contemporaneamente i tasti **Volume Su** e **Accensione** per avviare in modalità recovery TWRP.

4. **Backup del Sistema Attuale (Opzionale ma Raccomandato)**:
    - In TWRP, vai su **Backup**.
    - Seleziona tutte le partizioni (Sistema, Dati, Boot) e crea un backup sulla scheda microSD o sulla memoria interna.
    - Questo backup ti permetterà di ripristinare lo stato attuale del sistema in caso di problemi.

5. **Wipe (Pulizia delle Partizioni)**:
    - Prima di installare LineageOS, è necessario eseguire un wipe delle partizioni principali.
    - In TWRP, vai su **Wipe** > **Advanced Wipe**.
    - Seleziona **Dalvik/ART Cache**, **System**, **Data**, e **Cache**.
    - Esegui il wipe trascinando il cursore nella parte inferiore dello schermo.

6. **Installazione della ROM LineageOS**:
    - Torna al menu principale di TWRP e seleziona **Install**.
    - Naviga fino alla scheda microSD e seleziona il file `.zip` di LineageOS.
    - Conferma l'installazione trascinando il cursore nella parte inferiore dello schermo.
    - Attendi il completamento del processo di installazione.

7. **Installazione delle GApps (Opzionale)**:
    - Se hai scaricato le GApps e desideri installarle, ripeti il processo di installazione selezionando il file `.zip` delle GApps dalla scheda microSD.

8. **Riavvio del Sistema**:
    - Una volta completata l'installazione, torna al menu principale di TWRP e seleziona **Reboot** > **System**.
    - Il tablet si riavvierà in LineageOS.

9. **Prima Configurazione**:
    - Segui le istruzioni a schermo per configurare LineageOS.
    - Se hai installato le GApps, ti verrà chiesto di accedere al tuo account Google durante questa fase.
  
### 4. Preparazione del Tablet

1. **Prima Configurazione del Tablet**:
    - Dopo il riavvio in LineageOS, segui la procedura di configurazione iniziale.
    - **Imposta la lingua** e il **fuso orario** corretti.
    - Se hai installato le GApps, accedi al tuo account Google quando richiesto.
    - Salta le impostazioni non necessarie come l'aggiunta di un'impronta digitale o l'impostazione di un PIN, poiché il tablet sarà utilizzato come dashboard.

2. **Connessione alla Rete Wi-Fi**:
    - Vai su **Impostazioni** > **Wi-Fi**.
    - Connettiti alla rete Wi-Fi di casa per garantire che il tablet sia sempre online e possa comunicare con il server di Home Assistant.

3. **Aggiornamento del Sistema**:
    - Verifica che LineageOS sia aggiornato all'ultima versione disponibile.
    - Vai su **Impostazioni** > **Sistema** > **Aggiornamenti**.
    - Se sono disponibili aggiornamenti, scaricali e installali.

4. **Impostazione del Tablet in Modalità Sviluppatore**:
    - Vai su **Impostazioni** > **Info sul tablet**.
    - Tocca ripetutamente su **Numero Build** fino a quando appare il messaggio "Ora sei uno sviluppatore".
    - Torna al menu delle impostazioni e vai su **Opzioni sviluppatore**.
    - Abilita **Debug USB** e, se non è già stato fatto, **Sblocco OEM**. Queste opzioni ti permetteranno di eseguire ulteriori configurazioni via ADB se necessario.

5. **Ottimizzazione delle Prestazioni**:
    - Disattiva eventuali animazioni di sistema per migliorare le prestazioni del tablet:
        - Vai su **Impostazioni** > **Opzioni sviluppatore**.
        - Imposta **Scala animazione finestra**, **Scala animazione transizione** e **Scala durata Animator** su **Off**.

6. **Disattivazione del Blocco Schermo**:
    - Per un'esperienza più fluida, disattiva il blocco schermo:
        - Vai su **Impostazioni** > **Sicurezza** > **Blocco schermo**.
        - Seleziona **Nessuno**.

7. **Impostazione della Luminosità**:
    - Per ottimizzare la visibilità della dashboard, imposta la luminosità su un livello fisso:
        - Vai su **Impostazioni** > **Display** > **Luminosità**.
        - Disabilita la **Luminosità adattiva** e imposta la luminosità su un livello medio-alto.

8. **Abilitazione della Modalità Non Disturbare**:
    - Per evitare notifiche e suoni indesiderati, abilita la modalità Non Disturbare:
        - Vai su **Impostazioni** > **Suoni** > **Non disturbare**.
        - Configura le opzioni per assicurarti che il tablet rimanga silenzioso durante il funzionamento.

9. **Installazione di un File Manager**:
    - Se non è già presente, installa un file manager dal Google Play Store (se le GApps sono installate) o da un file APK, per facilitare la gestione dei file sul dispositivo.

10. **Test di Stabilità**:
    - Prima di procedere con ulteriori configurazioni, usa il tablet per qualche minuto per assicurarti che tutto funzioni correttamente e che non ci siano problemi di stabilità.
   




# Risorse Utili
Forum XDA Custom ROM: https://xdaforums.com/t/rom-unofficial-lineageos-14-1-lenovo-tb-x103f-7-1-2.3878883/
