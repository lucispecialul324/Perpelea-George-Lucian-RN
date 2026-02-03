# README – Etapa 6: Analiza Performantei, Optimizarea si Concluzii Finale

**Disciplina:** Retele Neuronale  
**Institutie:** POLITEHNICA Bucuresti – FIIR  
**Student:** Perpelea George-Lucian  
**Link Repository:** https://github.com/lucispecialul324/Perpelea-George-Lucian-RN


## 1. Actualizarea Aplicatiei Software (Maturizarea Proiectului)

In aceasta etapa finala, aplicatia a fost rafinata prin testarea interactiva a predictiilor in VI-ul principal si ajustarea pragurilor de recunoastere direct in interfata LabVIEW pentru a imbunatati precizia.

### Tabel Modificari Aplicatie Software (Etapa 6)

| Componenta         | Stare Etapa 5      | Modificare Etapa 6          | Justificare                                                    |
|--------------------|--------------------|-----------------------------|----------------------------------------------------------------|
| **Prag Decizie**   | 0.5 (Standard)     | Ajustat la 0.7              | Eliminarea predictiilor nesigure cand desenul este ambiguu.    |
| **Interfata (UI)** | Afisare simpla     | Adaugat indicatori vizuali  | Permite monitorizarea gradului de incredere al retelei.        |
| **Testare Date**   | Testare pe set fix | Testare manuala interactiva | Verificarea robustetii sistemului la variatii de scris manual. |
| **WebService**     | Inexistent         | Integrare **NI WEB SERVER   | Expunerea variabilelor globale pentru monitorizare externa.    | 

## 2. Optimizarea Parametrilor si Experimentare

### 2.1 Tehnica "Pixelului Critic"
Pentru a imbunatati precizia, am identificat manual "pixelii critici" – zone din grila 9x7 care au o pondere determinanta in stratul ascuns al retelei.
* **Exemplu:** Pentru a distinge cifrele '8' si '0', pixelii din centrul grilei devin critici. Daca acesti pixeli nu sunt activi, reteaua reevalueaza predictia catre '0', chiar daca forma exterioara este similara.

### 2.2  Tabel Experimente de Optimizare (Ajustari manuale)

| Exp# | Modificare Parametri      | Rezultat Observat        | Observatii                                                   |
|------|---------------------------|--------------------------|--------------------------------------------------------------|
| 1    | 10 Hidden Neurons, LR=0.1 | Eroare medie ridicata    | Reteaua nu are suficienta "memorie" pentru detalii.          | 
| 2    | **30 Hidden Neurons**     | **Eroare scazuta rapid** | **Cea mai buna configuratie pentru grila 9x28.**             |
| 3    | Learning Rate 0.05        | Convergenta lina         | Stabilitate mai mare, dar necesita mai multe iteratii.       |
| 4    | Creștere iterații (Teach) | Eroare finala minima     | S-a obtinut o potrivire mai buna a sabloanelor.              |
| 5    | Web API Debugging         | Erore -67003 (SSE2)      | Rezolvata prin dezactivarea optimizarii SSE2 in proprietati. |


**Justificare:**
S-a ales configuratia cu 30 de neuroni in stratul ascuns deoarece aceasta a permis distingerea literelor complexe fara a incetini procesarea in timp real a celor 4 sub-matrice.


## 3. Analiza Detaliata a Performantei

### 3.1 Analiza erorilor prin testare manuala
Analiza performantei a fost realizata prin desenarea repetata a caracterelor in interfata si observarea raspunsului retelei:

* **Puncte forte:** Cifrele (1, 4, 7) si literele cu linii drepte (L, T) sunt recunoscute aproape perfect.
* **Puncte slabe:** Exista confuzii intre caractere cu forme similare, precum cifra '8' si litera 'B', sau cifra '0' si litera 'O'.
* **Concluzie testare:** Grila de 9x7 pentru fiecare caracter este suficienta pentru majoritatea simbolurilor, dar necesita o desenare ingrijita pentru caracterele rotunjite.

### 3.2 Analiza Exemplelor GRESITE (Observatii manuale)

| Nr | Caracter desenat | Predictie eronata | Cauza identificata                                   |
|----|------------------|-------------------|------------------------------------------------------|
| 1  | Litera 'E'       | Litera 'F'        | Linia orizontala de jos nu a fost detectata.         |
| 2  | Cifra '8'        | Cifra '0'         | Zona de mijloc a fost lasata prea libera.            |
| 3  | Litera 'B'       | Cifra '8'         | Similitudine structurala mare la rezolutie mica.     |
| 4  | Litera 'A'       | Litera 'H'        | Varful literei nu a fost unit corect.                |
| 5  | Cifra '5'        | Litera 'S'        | Unghiurile rotunjite au fost interpretate ca litera. |

### 3.3 Implementarea Web Service
O directie cheie in aceasta etapa a fost expunerea rezultatelor prin **LabVIEW Web Services**:
* **Problema:** La pornirea serverului de debug, a aparut eroarea `-67003` (SSE2 optimization failure).
* **Solutie:** Navigarea in *Web Service Properties -> Advanced* si bifarea optiunii **Disable SSE2 optimization**.
* **Rezultat:** Datele din variabilele globale sunt acum accesibile printr-un URL dedicat, permitand monitorizarea sistemului dintr-un browser web.


## 4. Concluzii Finale si Lectii Invatate

### 4.1 Evaluarea Performantei Finale
Sistemul SIA implementat in LabVIEW reuseste sa prezica setul de 4 caractere cu o acuratete vizuala de peste 80%. Timpul de executie este optim, permitand o interactiune fluida cu utilizatorul pe matricea booleana.

### 4.2 Limitari Identificate
1. **Rezolutia:** Anumite litere si cifre sunt greu de diferentiat la o rezolutie de 9x7 pixeli per caracter.
2. **Centrarea:** Predictia depinde de pozitionarea corecta a desenului in interiorul sub-array-ului selectat.

### 4.3 Directii de Dezvoltare
* Extinderea bazei de date de antrenare cu mai multe variante de scris pentru fiecare litera si cifra.
* Implementarea unei logici de pre-procesare pentru centrarea automata a pixelilor activi.

### 4.4 Lectii Invatate
* **LabVIEW RN:** Am inteles fluxul de lucru cu clasa Neural Network (Create -> Teach -> Calculate Response).
* **Calitatea Datelor:** Am observat ca succesul predictiei depinde 90% de cat de clar sunt generate datele in GenerareDate.vi.
* **Parametri:** Am invatat ca un numar prea mare de neuroni poate duce la memorarea setului de date (overfitting) in loc de invatarea formelor.


## 5. Structura Finala Repository

Perpelea-George-Lucian-RN/
├── README.md                         # Fisierul principal de prezentare
├── config/                           # Parametrii retelei (iteratii, LR, praguri)
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

