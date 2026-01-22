# README – Etapa 4: Arhitectura Completa a Aplicatiei SIA bazata pe Retele Neuronale

**Disciplina:** Retele Neuronale
**Institutie:** POLITEHNICA Bucuresti – FIIR
**Student:** Perpelea George-Lucian
**Link Repository GitHub:** https://github.com/lucispecialul324/Perpelea-George-Lucian-RN
**Data:** 18/12/2025

## Scopul Etapei 4

Aceasta etapa corespunde punctului **5. Dezvoltarea arhitecturii aplicatiei software bazata pe RN**.
Va prezint un **SCHELET COMPLET si FUNCTIONAL** al intregului Sistem cu Inteligenta Artificiala (SIA) realizat in LabVIEW.

**Stadiul curent:**
- Modulele de generare date si antrenare sunt functionale.
- VI-ul Principal (Main Interface) ruleaza end-to-end (preia desenul -> segmenteaza -> apeleaza reteaua -> afiseaza rezultatul).
- Arhitectura retelei este definita si compilata, permitand salvarea/incarcarea ponderilor.

## 1. Tabelul Nevoie Reala -> Solutie SIA -> Modul Software

| **Nevoie reala concreta**                             | **Cum o rezolva SIA-ul vostru**                         | **Modul software responsabil**            |
|-------------------------------------------------------|---------------------------------------------------------|-------------------------------------------|
| Digitalizarea rapida a formularelor completate manual | Recunoastere optica a caracterelor (OCR) desenate pe o  | VI Principal (Segmentare) + Module RN     |
| manual cu caractere atipice                           | grila, cu segmentare automata a 4 simboluri simultan    | (Cifre/Litere/Simboluri)                  |
|-------------------------------------------------------|---------------------------------------------------------|-------------------------------------------|
| Flexibilitate in recunoasterea tipului de data        | Selector de context (Enum) care schimba dinamic reteaua | Logica de Control (Case Structure) + EnumS|
| (Cifra vs Litera)                                     | neuronala activa pentru a evita confuziile (ex: 0 vs O) |                                           |
|-------------------------------------------------------|---------------------------------------------------------|-------------------------------------------|

## 2. Contributia Voastra Originala la Setul de Date – 100% Original

Deoarece proiectul utilizeaza o grila de rezolutie non-standard (9x7 pixeli per caracter) specifica acestei implementtri hardware/software, nu s-au putut utiliza seturi de date publice (precum MNIST).

### Contributia originala la setul de date:

**Total observatii finale:** 46 sample-uri per cele 3 categorii
**Observatii originale:** 100%

**Tipul contributiei:**

[ ] Date generate prin simulare fizica
[ ] Date achizitionate cu senzori proprii
[X] **Etichetare/adnotare manuala si Generare prin interfata proprie**
[ ] Date sintetice prin metode avansate

**Descriere detaliata:**
Datele au fost create integral folosind generatoarele dedicate dezvoltate in Etapa 3 (`GenerareDateCifre.vi`, `GenerareDateLitere.vi`, etc.).
Am desenat manual fiecare caracter pe matricea de 9x7 pixeli. Fiecare vector de 63 de elemente (booleni) a fost salvat in fisiere binare personalizate (`.bin`), asigurand o potrivire perfecta intre datele de antrenare si cele de intrare din aplicatia finala.

**Locatia codului:** `src/GenerareDate/*.vi`
**Locatia datelor:** `data/*.bin`

**Dovezi:**
- Capturi de ecran ale VI-urilor de generare in `docs/screenshots/`
- Fisierele binare generate in folderul `data/`

## 3. Diagrama State Machine a Intregului Sistem

Diagrama de stari a fost implementata folosind arhitectura standard LabVIEW (FOR LOOP + Event Structure / Case Structure).

**Locatie diagrama:** `docs/state_machine.png`

### Justificarea State Machine-ului ales:

Am ales o arhitectura bazata pe evenimente (Event Driven) combinata cu flux de date secvential pentru procesarea imaginii.
Proiectul necesita interactiune directa cu utilizatorul (desenare), urmata de o procesare rapida la cerere.

**Starile principale sunt:**
1.  **IDLE / WAIT FOR EVENT:** Sistemul asteapta ca utilizatorul sa deseneze pe grid-ul de 9x28 sau sa schimbe tipul de date din Enum (Cifre/Litere).
2.  **SEGMENTATION:** La apasarea butonului de executie, matricea mare (9x28) este taiata (folosind `Array Subset`) in 4 sub-matrici de 9x7.
3.  **LOAD_CONTEXT:** In functie de Enum-ul selectat, se incarca fisierul de ponderi corespunzator (ex: `DateCifre.bin` sau `DateLitere.bin`).
4.  **INFERENCE:** Se ruleaza sub-VI-ul de predictie de 4 ori (pentru fiecare segment), calculand probabilitatile.
5.  **DISPLAY:** Se concateneaza cele 4 rezultate intr-un string si se afiseaza pe Front Panel.

Starea de **Eroare** este gestionata implicit prin mecanismul de "Error Cluster" din LabVIEW – daca un fisier `.bin` lipseste, executia se opreste controlat.

## 4. Scheletul Complet al celor 3 Module

Sistemul este complet modularizat folosind SubVI-uri.
|-------------------------|-----------------------------------------------------------------------------|-----------------------------------------------------|
| **Modul**               | **Implementare LabVIEW**                                                    | **Status Functional (Etapa 4)**                     |
|-------------------------|-----------------------------------------------------------------------------|-----------------------------------------------------|
| **1. Data Acquisition** | `GenerareDateCifre.vi`, `GenerareDateLitere.vi`, `GenerareDateSimboluri.vi` | **Functional:** Permite desenarea manuala si        |
|                         |                                                                             | salvarea in fisiere `.bin`.                         |
|-------------------------|-----------------------------------------------------------------------------|-----------------------------------------------------|
| **2. Neural Network**   | `Antrenare neuroni ghicire numere.vi` (si echivalentele)                    | **Definit:** Arhitectura (Input 63 -> Hidden ->     |
|                         |                                                                             | Output) este creata. Antrenarea ruleaza,            |
|                         |                                                                             | ponderile se salveaza.                              |
|-------------------------|-----------------------------------------------------------------------------|-----------------------------------------------------|
| **3. UI / Integration** | `Antrenare neuroni ghicire cifre_litere_simboluri_grupuri de simboluri.vi`  | **Functional:** Interfata 9x28 preia input,         |
|                         |                                                                             | segmenteaza si afiseaza rezultatul (chiar daca      |
|                         |                                                                             | predictia nu e inca perfecta).                      |
|-------------------------|-----------------------------------------------------------------------------|-----------------------------------------------------|

#### Detalii per modul:

**Modul 1: Generare Date**
- Ruleaza independent.
- Output: Fisiere binare in folderul `data/` care contin vectori de `Cluster (Array Bool, String Label)`.

**Modul 2: Reteaua Neuronala**
- Foloseste toolkit-ul intern pentru crearea straturilor MLP.
- SubVI-ul `Create Network.vi` defineste topologia.
- SubVI-ul `Teach.vi` ajusteaza ponderile (Backpropagation).

**Modul 3: Interfata Principala (UI)**
- **Input:** Control grafic 2D Array (Boolean) modificabil cu mouse-ul.
- **Selector:** Enum (Ring Control) pentru comutarea contextului.
- **Output:** Indicator String pentru rezultatul final.

## 5. Structura Repository-ului la Finalul Etapei 4

Perpelea-George-Lucian-RN/
├── data/
│   ├── DateCifre.bin          # Date originale generate
│   ├── DateLitere.bin
│   └── DateSimboluri.bin
├── src/
│   ├── GenerareDate/          # MODUL 1
│   │   ├── GenerareDateCifre.vi
│   │   ├── GenerareDateLitere.vi
│   │   └── GenerareDateSimboluri.vi
│   ├── Antrenare/             # MODUL 2 (SubVI-uri RN)
│   │   ├── Antrenare neuroni ghicire numere.vi
│   │   ├── Antrenare neuroni ghicire litere.vi
│   │   ├── Antrenare neuroni ghicire simboluri.vi
│   │   └── SubVIs/ (Create Network, Teach, etc.)
│   └── VI_Final/              # MODUL 3 (App Principala)
│       └── Antrenare neuroni ghicire cifre_litere_simboluri_grupuri de simboluri.vi
├── docs/
│   ├── state_machine.png      # Diagrama de stari (Imagine exportata din LabVIEW sau desenata)
│   ├── screenshots/           # Capturi cu interfata
│   └── etapa4_arhitectura.md
├── README.md
└── README_Etapa4_Arhitectura_SIA.md  # Acest fisier