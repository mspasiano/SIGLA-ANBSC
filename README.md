# SIGLA-ANBSC

**SIGLA-ANBSC** è una suite di applicazioni web dedicate alla gestione e alla stampa di dati contabili, basata su un'architettura containerizzata. Il progetto utilizza servizi basati su **WildFly**, **Spring Boot**, **Oracle XE** e un'interfaccia web in Angular.

## 🧱 Struttura dei servizi

Il progetto è composto dai seguenti container:

| Servizio         | Descrizione                                                             |
|------------------|--------------------------------------------------------------------------|
| `sigla-oracle`   | Database Oracle 18 XE, inizializzato con script custom (`initdb-oracle`) |
| `sigla-wildfly`  | Backend applicativo Java EE su WildFly (SIGLA Main)                      |
| `sigla-ng`       | Frontend Angular, personalizzabile tramite `custom.css` e `it.json`      |
| `sigla-print`    | Servizio di stampa/reportistica Spring Boot                              |

---

## 🚀 Avvio rapido

### 1. Clona il repository

```bash
git clone https://github.com/mspasiano/SIGLA-ANBSC.git
cd SIGLA-ANBSC
```

### 2. Prepara l’ambiente
Assicurati di avere:

- Docker >= 20.x
- Docker Compose >= 1.29
- Almeno 8GB di RAM disponibili

Verifica che i file seguenti esistano (o personalizzali):

```
initdb-oracle/       → script SQL per inizializzare il database Oracle
custom.css           → personalizzazioni UI Angular
it.json              → traduzioni interfaccia italiana
```

### 3. Avvia i container

```bash
docker compose up --build
```

Il primo avvio può richiedere alcuni minuti per inizializzare Oracle.

---

## 🌐 Accesso ai servizi

| Servizio      | URL                        | Note                                                                             |
|---------------|----------------------------|----------------------------------------------------------------------------------|
| Frontend UI   | http://localhost:9000      | Interfaccia principale SIGLA, al primo accesso utilizzare l'utenza **ENTE:ENTE** |
| Backend API   | http://localhost:8080/SIGLA| WildFly SIGLA backend                                                            |
| Reportistica  | http://localhost:8081      | Servizio stampa (SIGLA Print)                                                    |
| Oracle DB     | `localhost:1521/XEPDB1`    | SID/PDB predefinito                                                              |

---

## 🔐 Credenziali di default

| Servizio   | Utente  | Password      |
|------------|---------|---------------|
| Oracle DB  | `SIGLA` | `siglapw`     |
| Oracle SYS | `SYS`   | `MyPassword123` (via `ORACLE_PASSWORD`) |

---

## ⚙️ Volumi e persistenza

I dati del database Oracle sono inizializzati tramite script in `./initdb-oracle`.

Se desideri rendere persistenti i dati:

```yaml
volumes:
  - oracle_data:/opt/oracle/oradata  # Da aggiungere a sigla-oracle
```

E in fondo al file:

```yaml
volumes:
  oracle_data:
```

---

## 📁 Customizzazione UI

Puoi modificare l'interfaccia utente tramite i file montati:

- `custom.css`: stile personalizzato Angular
- `it.json`: localizzazione italiana

---

## 🛠️ Debug & Log

Ogni container è configurato con log rotazionali (`max-size`, `max-file`), consultabili con:

```bash
docker logs -f sigla-wildfly
docker logs -f sigla-ng
docker logs -f sigla-print
docker logs -f sigla-oracle
```

---

## 🧪 Healthchecks

Ogni servizio critico è monitorato per l'avvio corretto tramite `healthcheck`, impedendo il bootstrap prematuro degli altri servizi.

---

## 🙋‍♂️ Supporto

Per segnalazioni, problemi o contributi:

1. Apri un'Issue nel repository GitHub
2. Contatta il maintainer @mspasiano 

---
