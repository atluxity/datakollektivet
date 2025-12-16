# Forslag til Tech Autonomy Stack

Målet er å bygge et komplett, selvhostet økosystem som inneholder alt en generisk organisasjon eller selskap skal trenge for identitet, kommunikasjon, datahåndtering og sikkerhet – uten avhengighet til Microsoft, Google, Amazon eller andre hyperscalere. Fokus ligger på **applikasjonslaget**. Underliggende virtualisering eller containerisering forventes, men kan løses med mange frie løsninger. Det er ikke fokuset her.

Det er tenkt på behovene til de som skal operere i organisasjonen og det er tenkt på **NSM sine grunnprinsipper**: oversikt, sikre, oppdage og håndtere.

---

## Behovene i applikasjonslaget

Når en organisasjon skal drive selvhostet infrastruktur på en ryddig og sikker måte, må flere funksjoner dekkes:

- identitet og autentisering  
- e-post (IMAP/SMTP)  
- webmail  
- spam- og malwarekontroll  
- fillagring og deling  
- sanntids dokumentredigering  
- passord- og hemmelighetshåndtering  
- SIEM og sårbarhetsdeteksjon  
- oversikt over systemer og tjenester  
- backup av applikasjonsdata  
- ekstern og intern nett­tilgang via en sikker front (reverse proxy)  

Dette er **applikasjonslaget**: tjenestene som brukes i hverdagen og som er kritiske for identitet, drift og data.

---

# NSM Grunnprinsipp 1: Kontinuerlig oversikt

- identitetsoversikt  
- system- og tjenesteoversikt  
- kontroll på sikkerhetskritiske applikasjoner  
- dokumenterte avhengigheter  

# NSM Grunnprinsipp 2: Sikre

- sterk autentisering  
- tilgangskontroll  
- tjenester som er hardnet og eksponert på en trygg måte  
- backup som ikke er sirkulært avhengig av samme infrastruktur  
- passord og hemmeligheter håndtert separat fra identitetsplattformen  

# NSM Grunnprinsipp 3: Oppdage

- logginnsamling på tvers av tjenester  
- deteksjon av avvik og angrep  
- overvåkning av sårbarheter og konfigurasjonsendringer  
- varsling når noe skjer  

# NSM Grunnprinsipp 4: Håndtere

- rask gjenoppretting  
- tydelig rekkefølge for restore  
- uavhengig backupserver  
- sporbarhet og hendelseshistorikk  

---

# Programvaren som oppfyller behovene

Valget av programvare er basert på modenhet, stabilitet, fri lisens og evne til å passe inn i en modulær arkitektur. Her er komponentene og hvorfor de er valgt.

---

## Identitet og autentisering

### **FreeIPA**
Sentral katalog for brukere, grupper, maskiner og nøkler. Oppfyller NSM GP1 og GP2 ved å gi oversikt og kontroll.

### **Keycloak**
Moderne SSO og MFA for alle webtjenester. Beskytter applikasjoner og gir sentralisert tilgangsstyring.

---

## E-post

### **Postfix**
Stabil, moden MTA for inn- og utgående e-post.

### **Dovecot**
IMAP-server med god ytelse og sikkerhet.

### **Rspamd + ClamAV**
Håndterer spam, phishing og malware – viktig for å redusere angrepsflate.

### **Roundcube**
En moden, stabil og ren webmail-klient som kun bruker åpne standarder. Det integrerer sømløst med Postfix og Dovecot, er lett å sikre bak SSO, og unngår kompleksiteten og bindingene som følger med tyngre groupware-løsninger.

---

## Filer og dokumenter

### **Nextcloud**
Selvhostet erstatning for Google Drive. Holder data under egen kontroll. Potensielt finnes det forks av Nextcloud som kan være verdt å vurdere.

### **Collabora Online**
Sanntids dokumentredigering basert på LibreOffice.

---

## Passord og hemmeligheter

### **Vaultwarden**
Passordhvelv. Viktig at denne ikke er avhengig av identitetsplattformen slik at man unngår sirkulær avhengighet.

---

## Overvåkning og deteksjon

### **Wazuh**
Samler logger, finner sårbarheter, utfører filintegritetsmonitorering og IDS-lignende deteksjon. Oppfyller hele Grunnprinsipp 3.

---

## Inventar og tjenesteoversikt

### **NetBox**
CMDB for tjenester, IP-adresser, maskiner og avhengigheter. Gir reell oversikt i tråd med Grunnprinsipp 1.

---

## Backup

### **Restic + Autorestic**
Deklarativ applikasjonsbackup, kryptert og enkel å gjenopprette. Kombinert med en egen fysisk backupserver for å unngå felles feil og sirkulær avhengighet. Oppfyller Grunnprinsipp 2 og 4.

---

## Front for alle tjenester

### **nginx (reverse proxy)**
Én sikker inngang til alle tjenester, med TLS-håndtering, hardening, logging, rate limiting og mulighet for å servere en enkel statisk hjemmeside.

---

# En samlet applikasjonsplattform

Resultatet er et helhetlig applikasjonslag der alle kritiske funksjoner er dekket med fri programvare:

- identitet  
- e-post  
- filer  
- dokumenter  
- passord  
- overvåkning  
- inventar  
- backup  
- web-front  

Alt dette er løst uten å bruke tjenester fra Microsoft, Google, Amazon eller andre hyperscalere. Arkitekturen er modulær, sikker og kan rebuildes fra bunnen av ved behov.

Dette gir full kontroll, bedre sikkerhet, og en infrastruktur som kan vokse sammen med organisasjonens behov – i tråd med NSM sine prinsipper og uten eksterne bindinger.
