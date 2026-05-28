# ☣️ Cyber Guardians: Malware Analysis Operativa & Host-Based Hunting

**Team:** Cyber Guardians | **Contesto:** Build Week 3 @ EPICODE Institute of Technology[cite: 2]
Questo progetto documenta un'attività intensiva di analisi forense, triage sistemistico e threat hunting host-based[cite: 3]. L'obiettivo è stato quello di analizzare, monitorare e mitigare le minacce poste da un campione malevolo a runtime, emulando i processi di investigazione tipici di un SOC Tier 1 al fine di mappare le tecniche di persistenza ed evasione e procedere alla bonifica dell'host[cite: 3].

---

## 🎯 Obiettivi e Scenario

*   **Perimetro d'azione:** Ambiente sandbox isolato (FlareVM)[cite: 3], infrastruttura di rete monitorata tramite gateway Linux[cite: 3], workstation CyberOps Linux[cite: 3], Security Onion (Sguil, Kibana, capME)[cite: 3, 4].
*   **Sfida:** Effettuare l'analisi statica e dinamica del rogue software AdwereCleaner.exe[cite: 3], tracciare gli artefatti generati a sistema[cite: 3], investigare flussi clandestini di esfiltrazione dati (DNS Tunneling, SQLi, FTP)[cite: 3, 4] e sviluppare regole YARA custom anti-evasione[cite: 3].

---

## 🛠️ Tecnologie e Strumenti Utilizzati

<!-- INCOLLA QUI I LOGHI DEI TOOL COME HAI FATTO NELLA BW2 -->
<!-- Esempio: FlareVM, Process Monitor, YARA, Splunk/Kibana, Wireshark, Immunity/Mona -->

---

## ⚙️ Dettagli delle Fasi Operative

### 1. Malware Analysis Operativa (AdwereCleaner.exe)
*   **Analisi Statica Iniziale:** Acquisizione degli hash strutturali (`MD5`, `SHA1`, `SHA256`)[cite: 3], verifica della validità della firma Authenticode (WAT Software Rotterdam)[cite: 3] ed estrazione delle stringhe/API core (`KERNEL32.dll`, `ADVAPI32.dll`) per mappare le capacità potenziali del file[cite: 3].
*   **Analisi Dinamica Controllata:** Esecuzione del campione in FlareVM isolata e monitoraggio a runtime degli eventi tramite **Process Monitor (ProcMon)**[cite: 3], documentando la logica intimidatoria dell'interfaccia scareware[cite: 3].
*   **Payload & Persistenza:** Identificazione dello sdoppiamento del processo tramite l'allocazione dinamica del file secondario latente `6AdwCleaner.exe` in `AppData\Local`[cite: 3]. Individuazione del vettore di persistenza automatica all'avvio mediante inserimento di Run keys nel Registro di Windows (`HKCU` con parametro `-auto`)[cite: 3] ed estrazione delle prove forensi nei database **BAM (Background Activity Moderator)** e `FeatureUsage`[cite: 3].

### 2. Incident Response & Hunting: DNS Tunneling & SQL Injection
*   **Vettore di Compromissione HTTP:** Investigazione della telemetria Zeek tramite dashboard Kibana[cite: 4]. Identificazione di un attacco **SQL Injection UNION-based** riuscito verso l'host interno `209.165.200.235`[cite: 4], mirato all'estrazione di record sensibili di carte di credito confermato da una risposta `200 OK` analizzata su **capME**[cite: 4].
*   **DNS Covert Channel:** Rilevamento di anomalie infrastrutturali nel traffico DNS verso finti sottodomini ad alta entropia[cite: 4]. Esportazione delle query in formato `.csv`[cite: 4] ed esecuzione della decodifica tramite utility di terminale Linux (`xxd -r -p`), isolando e ricostruendo l'esfiltrazione clandestina di un documento classificato come `CONFIDENTIAL`[cite: 4].
*   **Exfiltration Hunting:** Analisi delle sessioni non cifrate tramite i log `bro_ftp` ed estrazione dal flusso di rete del canale dati originario (**FTP_DATA** su porta `20`) per tracciare la destinazione finale e la pipeline del data breach[cite: 4].

### 3. Threat Hunting avanzato con YARA Rules
*   **Scrittura Firme Custom:** Progettazione di una regola YARA di produzione ottimizzata per contrastare le tecniche di Defense Evasion (tecniche MITRE `T1012` e `T1082`)[cite: 3].
*   **Pattern Matching:** Configurazione della logica di rilevamento integrando modificatori di stringa (`ascii`, `wide`, `nocase`)[cite: 3] e pattern esadecimali legati a sequenze di memory injection, garantendo l'efficacia della regola anche in caso di masquerading su processi di sistema legittimi (`svchost.exe`, `taskhost.exe`) o di alterazione dell'hash crittografico[cite: 3].

### 4. Network Forensics & Analisi dei Servizi Linux
*   **Estrazione Eseguibile da PCAP:** Ispezione a livello di pacchetto di transazioni di rete non cifrate in Wireshark[cite: 3]. Utilizzo della funzione *Follow TCP Stream* ed *Export Objects* per ricostruire e salvare sul file system locale il malware binario PE32+ `W32.Nimda.Amm.exe` trasferito via HTTP[cite: 3].
*   **Triage Sistemistico Linux:** Triage e mappatura delle gerarchie dei processi (*parent-child relationship*) tramite comandi CLI avanzati (`ps -elf`, `ps -ejH`)[cite: 3]. Correlazione dei socket di rete in ascolto (`netstat -tunap`)[cite: 3] e validazione applicativa di banner grabbing tramite diagnostica Telnet[cite: 3].

---

## 📂 Risorse del Progetto

*   📄 **Report Tecnico Finale - Malware Analysis & Incident Investigation (PDF):** Documentazione SOC completa di tutte le fasi di assessment, stringhe estratte, timeline d'infezione, indicatori IOC/IOA mappati e piano dettagliato di remediation applicato tramite snapshot[cite: 3, 4].

## 🎯 Conclusioni Operative

Il progetto ha permesso di consolidare le competenze fondamentali di un analista SOC nell'identificazione e nella mitigazione di minacce host-based, dimostrando l'importanza di non affidarsi esclusivamente alla firma crittografica di un file, ma di analizzarne i reali indicatori comportamentali a runtime per garantire una bonifica completa e sicura dell'infrastruttura.
