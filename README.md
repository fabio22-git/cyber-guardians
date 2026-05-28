# Cyber Guardians 🛡️

### Malware Analysis Operativa & Host-Based Hunting | EPICODE (Build Week 3)

Benvenuti nella repository di **Cyber Guardians**, il progetto finale dedicato alla Malware Analysis e all'Host-Based Threat Hunting sviluppato durante la terza Build Week del Master in Cyber Security & Ethical Hacking di EPICODE.

L'obiettivo principale dell'attività è stato quello di analizzare in modo sicuro e controllato un campione di codice potenzialmente malevolo, ricostruendo le tattiche di persistenza ed evasione per elaborare contromisure efficaci di rilevamento sistemistico.

---

## 🛠️ Attività Tecniche Svolte

*   **Triage & Dynamic Analysis:** Analisi comportamentale del rogue software *AdwereCleaner.exe* all'interno di un ambiente isolato (**FlareVM**) instradato tramite gateway Linux, monitorando l'attività a runtime e la generazione di processi secondari tramite **Process Monitor (ProcMon)**.
*   **Registry & Evasion Analysis:** Identificazione degli indicatori di compromissione (IOC/IOA) e delle chiavi di Registro Run utilizzate per la persistenza automatica al login utente. Mappatura delle tattiche di Defense Evasion sul framework **MITRE ATT&CK** (tecniche `T1012` e `T1082`) e ispezione del database forense **BAM (Background Activity Moderator)**.
*   **Threat Hunting & Remediation:** Isolamento dei codici corrompenti (*badchars*) tramite comparazione iterativa dei bytearray con `mona.py`. Sviluppo, test e validazione di una regola custom **YARA** ottimizzata anti-evasione per l'intercettazione di varianti resistenti al cambio di hash. Stesura delle procedure di Incident Response e bonifica dell'host.

---

## 💻 Strumenti e Tecniche Utilizzate

*   **Ambienti di Analisi:** FlareVM, Kali Linux (Routing/NAT controllato)
*   **Log & Process Analysis:** Process Monitor (ProcMOn), WMIC, Tasklist
*   **Threat Hunting:** YARA Engine (Scansione ricorsiva via CLI)
*   **Integrità & Forensics:** Certutil (Hashing MD5/SHA256), Windows Registry Execution Artifacts (BAM, FeatureUsage)
*   **Frameworks:** MITRE ATT&CK Enterprise Matrix, Mona.py / Immunity Debugger

---

## 📂 Struttura del Progetto

*   `AdwereCleaner.exe`: Il campione iniziale di rogue software analizzato staticamente e dinamicamente.
*   `6AdwCleaner.exe`: Il payload secondario persistente estratto dal percorso *AppData\Local*.
*   `caccia_malware.yar`: La regola YARA custom strutturata con costanti testuali e pattern esadecimali.
*   `Report_Malware_Analysis.pdf`: La documentazione tecnica dettagliata con i log, la timeline d'infezione e le metriche di remediation.

---

## 🎯 Conclusioni Operative

Il progetto ha permesso di consolidare le competenze fondamentali di un analista SOC nell'identificazione e nella mitigazione di minacce host-based, dimostrando l'importanza di non affidarsi esclusivamente alla firma crittografica di un file, ma di analizzarne i reali indicatori comportamentali a runtime per garantire una bonifica completa e sicura dell'infrastruttura.
