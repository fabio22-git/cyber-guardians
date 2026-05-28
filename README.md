# ☣️ Cyber Guardians: Malware Analysis Operativa & Host-Based Hunting

**Team:** Cyber Guardians | **Contesto:** Build Week 3 @ EPICODE Institute of Technology

Questo progetto documenta un'attività intensiva di analisi forense, triage sistemistico e threat hunting host-based. L'obiettivo è stato quello di analizzare, monitorare e mitigare le minacce poste da un campione malevolo a runtime, emulando i processi di investigazione tipici di un SOC Tier 1 al fine di mappare le tecniche di persistenza ed evasione e procedere alla bonifica dell'host.

---

## 🎯 Obiettivi e Scenario

*   **Perimetro d'azione:** Ambiente sandbox isolato (FlareVM), infrastruttura di rete monitorata tramite gateway Linux, workstation CyberOps Linux, Security Onion (Sguil, Kibana, capME).
*   **Sfida:** Effettuare l'analisi statica e dinamica del rogue software AdwereCleaner.exe, tracciare gli artefatti generati a sistema, investigare flussi clandestini di esfiltrazione dati (DNS Tunneling, SQLi, FTP) e sviluppare regole YARA custom anti-evasione.

---

## 🛠️ Tecnologie e Strumenti Utilizzati

<p align="left">
  <img src="https://img.shields.io/badge/FLARE_VM-E6194B?style=for-the-badge&logo=windows&logoColor=white" alt="FlareVM" />
  <img src="https://img.shields.io/badge/PROCMON-000000?style=for-the-badge&logo=windows-terminal&logoColor=00FF00" alt="ProcMon" />
  <img src="https://img.shields.io/badge/YARA_ENGINE-34A853?style=for-the-badge&logo=google-cloud&logoColor=white" alt="YARA" />
  <img src="https://img.shields.io/badge/WIRESHARK-1679A7?style=for-the-badge&logo=wireshark&logoColor=white" alt="Wireshark" />
  <img src="https://img.shields.io/badge/SPLUNK_SIEM-F7A800?style=for-the-badge&logo=splunk&logoColor=black" alt="Splunk" />
  <img src="https://img.shields.io/badge/KALI_LINUX-557C94?style=for-the-badge&logo=kali-linux&logoColor=white" alt="Kali Linux" />
</p>

---

## ⚙️ Dettagli delle Fasi Operative

### 1. Malware Analysis Operativa (AdwereCleaner.exe)
*   **Analisi Statica Iniziale:** Acquisizione degli hash strutturali (`MD5`, `SHA1`, `SHA256`), verifica della validità della firma Authenticode (WAT Software Rotterdam) ed estrazione delle stringhe/API core (`KERNEL32.dll`, `ADVAPI32.dll`) per mappare le capacità potenziali del file.
*   **Analisi Dinamica Controllata:** Esecuzione del campione in FlareVM isolata e monitoraggio a runtime degli eventi tramite **Process Monitor (ProcMon)**, documentando la logica intimidatoria dell'interfaccia scareware.
*   **Payload & Persistenza:** Identificazione dello sdoppiamento del processo tramite l'allocazione dinamica del file secondario latente `6AdwCleaner.exe` in `AppData\Local`. Individuazione del vettore di persistenza automatica all'avvio mediante inserimento di Run keys nel Registro di Windows (`HKCU` con parametro `-auto`) ed estrazione delle prove forensi nei database **BAM (Background Activity Moderator)** e `FeatureUsage`.

### 2. Incident Response & Hunting: DNS Tunneling & SQL Injection
*   **Vettore di Compromissione HTTP:** Investigazione della telemetria Zeek tramite dashboard Kibana. Identificazione di un attacco **SQL Injection UNION-based** riuscito verso l'host interno `209.165.200.235`, mirato all'estrazione di record sensibili di carte di credito confermato da una risposta `200 OK` analizzata su **capME**.
*   **DNS Covert Channel:** Rilevamento di anomalie infrastrutturali nel traffico DNS verso finti sottodomini ad alta entropia. Esportazione delle query in formato `.csv` ed esecuzione della decodifica tramite utility di terminale Linux (`xxd -r -p`), isolando e ricostruendo l'esfiltrazione clandestina di un documento classificato come `CONFIDENTIAL`.
*   **Exfiltration Hunting:** Analisi delle sessioni non cifrate tramite i log `bro_ftp` ed estrazione dal flusso di rete del canale dati originario (**FTP_DATA** su porta `20`) per tracciare la destinazione finale e la pipeline del data breach.

### 3. Threat Hunting avanzato con YARA Rules
*   **Scrittura Firme Custom:** Progettazione di una regola YARA di produzione ottimizzata per contrastare le tecniche di Defense Evasion (tecniche MITRE `T1012` e `T1082`).
*   **Pattern Matching:** Configurazione della logica di rilevamento integrando modificatori di stringa (`ascii`, `wide`, `nocase`) e pattern esadecimali legati a sequenze di memory injection, garantendo l'efficacia della regola anche in caso di masquerading su processi di sistema legittimi (`svchost.exe`, `taskhost.exe`) o di alterazione dell'hash crittografico.

### 4. Network Forensics & Analisi dei Servizi Linux
*   **Estrazione Eseguibile da PCAP:** Ispezione a livello di pacchetto di transazioni di rete non cifrate in Wireshark. Utilizzo della funzione *Follow TCP Stream* ed *Export Objects* per ricostruire e salvare sul file system locale il malware binario PE32+ `W32.Nimda.Amm.exe` trasferito via HTTP.
*   **Triage Sistemistico Linux:** Triage e mappatura delle gerarchie dei processi (*parent-child relationship*) tramite comandi CLI avanzati (`ps -elf`, `ps -ejH`). Correlazione dei socket di rete in ascolto (`netstat -tunap`) e validazione applicativa di banner grabbing tramite diagnostica Telnet.

---

## 📂 Risorse del Progetto

*   📄 [**Report Tecnico Finale - Malware Analysis & Incident Investigation (PDF)**](Report_Malware_Analysis.pdf): Documentazione SOC completa di tutte le fasi di assessment, stringhe estratte, timeline d'infezione, indicatori IOC/IOA mappati e piano dettagliato di remediation applicato tramite snapshot.
