# Perpelea_George-Lucian_GRUPA_README_Proiect_RN.md

**Disciplina:** Retele Neuronale (RN)  
**Institutie:** Universitatea POLITEHNICA Bucuresti – FIIR  
**Student:** Perpelea George-Lucian  
**Link Repository GitHub:** https://github.com/lucispecialul324/Perpelea-George-Lucian-RN  
**Tag Versiune:** `v1.0-final-release`


## 1. Declaratia de Originalitate si Politica de Utilizare AI

Prin prezenta, declar ca proiectul „Sistem SIA pentru Recunoasterea Caracterelor in LabVIEW” reprezinta munca mea originala.
* **Contributie Date:** 100% din datele de antrenare au fost generate manual.
* **Depanare Tehnica:** Am utilizat asistenta AI pentru documentarea erorii de sistem `-67003` si pentru structurarea rapoartelor. 
* **Implementare:** Conceptul tehnic, arhitectura retelei si configurarea serverului web in LabVIEW au fost realizate integral de catre student.

## 2. Descrierea Proiectului si Obiective

Proiectul implementeaza un Sistem cu Inteligenta Artificiala (SIA) in LabVIEW pentru recunoasterea simultana a 4 caractere pe o matrice de **9x28**.

**Module Functionale:**
1. **Data Logging:** Achizitie si etichetare manuala a datelor originale.
2. **Modul RN:** Retea Feed-Forward optimizata cu 30 de neuroni in stratul ascuns.
3. **Web Service (IoT):** API local pentru monitorizarea predictiilor de pe orice dispozitiv din retea.
4. **UI (Interfata):** Dashboard interactiv cu feedback vizual si praguri de incredere ajustabile.

## 3. Analiza si Pregatirea Setului de Date (Etapa 3)

* **Segmentare:** Matricea 9x28 este impartita in 4 cadrane de 9x7 pixeli.
* **Preprocesare:** Transformarea sub-matricilor in vectori 1D (63 caracteristici).
* **Fisiere:** `DateCifre.bin`, `DateLitere.bin`, `DateSimboluri.bin` (format binar propriu).

## 4. Arhitectura SIA si Logica de Control

### 4.1 State Machine (Flow Aplicatie)
Aplicatia ruleaza intr-o masina de stari: **IDLE** (asteptare) -> **PROCESS** (segmentare) -> **INFERENCE** (calcul RN) -> **WEB UPDATE** (trimitere date catre API) -> **DISPLAY**.

### 4.2 Conceptul de „Pixel Critic”
Pentru a rezolva confuziile intre caractere similare (ex: '8' vs 'B' sau '0'), am implementat analiza **pixelilor critici**:
* Se monitorizeaza ponderile (weights) zonelor centrale ale matricii.
* Daca un „pixel critic” central nu este activat, reteaua penalizeaza scorul pentru caracterele inchise (ca '8') in favoarea celor deschise.



## 5. Antrenare, Optimizare si Web Service

### 5.1 Experimente de Optimizare
| Exp# | Configuratie | Accuracy | F1-Score | Observatii |
|:---:|:---:|:---:|:---:|:---:|
| 1 | 10 Neuroni | 68% | 0.62 | Capacitate de generalizare scazuta. |
| **3** | **30 Neuroni** | **82%** | **0.78** | **Configuratie OPTIMA - Stabilitate maxima.** |

### 5.2 Integrare Web Service (Etapa 6)
Am adaugat un layer de comunicatie prin **NI Web Server** pentru a expune rezultatele predictiei prin protocol HTTP.
* **Provocare:** Eroarea `-67003` (SSE2 optimization error) la pornirea serverului.
* **Solutie:** Dezactivarea optiunii **Enable SSE2 optimization** in proprietatile avansate ale Web Service-ului.
* **Functionalitate:** Permite citirea variabilelor globale ale proiectului dintr-un browser web extern.

## 6. Rezultate Finale si Analiza Performantei

* **Viteza de raspuns:** < 50ms (Inferenta instantanee).
* **Acuratete:** 82% (Peste pragul de 70% impus).
* **Puncte Forte:** Recunoastere excelenta a literelor cu linii drepte si a cifrelor clare.
* **Puncte Slabe:** Sensibilitate la centrarea desenului in interiorul cadranului de 9x7.

## 7. Instructiuni de Rulare

1. **Pornire Server:** Deschideti *NI Web Server Configuration* si asigurati-va ca statusul este „Running”.
2. **Start Proiect:** In LabVIEW Project Explorer, dati click dreapta pe `MyWebService` -> **Start**.
3. **Aplicatie:** Rulati `VI_Final/Antrenare neuroni ghicire cifre_litere_simboluri_grupuri de simboluri.vi`.
4. **Monitorizare:** Dati click dreapta pe VI-ul din Web Resources -> **Show Method URL** pentru a vedea rezultatele in browser.

## 8. Structura Repository (Tag: v1.0-final-release)

Perpelea-George-Lucian-RN/
├── README.md                         # Fisierul principal de prezentare
├── config/                           # Parametrii retelei (iterații, LR, praguri)
├── data/                             # Seturile de date originale (.bin)
│   ├── DateCifre.bin             
│   ├── DateLitere.bin            
│   └── DateSimboluri.bin         
├── docs/                             # Documentatia proiectului pe etape
│   ├── etapa3_analiza_date.md
│   ├── etapa4_arhitectura_sia.md
│   ├── etapa5_antrenare_model.md
│   ├── etapa6_optimizare_concluzii.md
│   ├── state_machine.png             
│   ├── demo/                         # Video demonstrativ (End-to-End)
│   └── screenshots/                  # Capturi Front Panel si Block Diagram
├── models/                           # Modelele antrenate
│   └── trained_model.bin             # Fisierul de greutati salvat (Final)
├── results/                          # Grafice de eroare si statistici finale
└── src/
    └── app/                          # Codul sursa functional LabVIEW
        ├── GenerareDate/             # Modulul de achizitie (Logging)
        │   ├── GenerareDateCifre.vi
        │   ├── GenerareDateLitere.vi
        │   └── GenerareDateSimboluri.vi
        ├── Antrenare/                # Modulul de training (RN)
        │   ├── Antrenare neuroni ghicire numere.vi
        │   ├── Antrenare neuroni ghicire litere.vi
        │   ├── Antrenare neuroni ghicire simboluri.vi
        │   └── SubVIs/               # Libraria Neural Network (API)
        └── VI_Final/                 # Modulul de interfata (UI)
            └── Antrenare neuroni ghicire cifre_litere_simboluri_grupuri de simboluri.vi