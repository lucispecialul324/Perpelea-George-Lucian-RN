# Perpelea_George-Lucian_GRUPA_README_Proiect_RN.md

**Disciplina:** Retele Neuronale (RN)  
**Institutie:** Universitatea POLITEHNICA Bucuresti – FIIR  
**Student:** Perpelea George-Lucian  
**Link Repository GitHub:**  https://github.com/lucispecialul324/Perpelea-George-Lucian-RN
**Tag Versiune:** `v0.6-optimized-final`

## 1. Declaratia de Originalitate si Politica de Utilizare AI

Prin prezenta, declar ca proiectul „Sistem SIA pentru Recunoasterea Caracterelor in LabVIEW” reprezinta munca mea originala.
* **Contributie Date:** 100% din datele de antrenare (fisierele .bin) au fost generate manual prin modulele proprii de achizitie si etichetare.
* **Utilizare AI:** Modelele de limbaj au fost utilizate exclusiv pentru asistenta in structurarea documentatiei si depanarea unor erori de logica. Conceptul tehnic, configurarea retelei si implementarea tuturor VI-urilor in LabVIEW au fost realizate integral de catre studentul Perpelea George-Lucian.

## 2. Descrierea Proiectului si Obiective

Proiectul implementeaza un Sistem cu Inteligenta Artificiala (SIA) dezvoltat in LabVIEW pentru recunoasterea a 4 caractere (cifre, litere si simboluri) introduse simultan pe o matrice booleana de **9 linii si 28 de coloane**.

**Module Functionale Obligatorii:**
1.  **Data Logging (Achizitie):** Module pentru desenarea, segmentarea si salvarea datelor originale.
2.  **Modul RN:** Retea neuronala Feed-Forward cu strat ascuns (Hidden Layer) pentru procesarea vectorilor 1D.
3.  **UI (Interfata):** Aplicatie integrata care ofera predictie in timp real si feedback vizual despre increderea modelului.


## 3. Analiza si Pregatirea Setului de Date (Etapa 3)

* **Origine:** 100% date originale generate programatic prin interactiune manuala.
* **Format:** Datele sunt stocate in format binar (`.bin`), continand vectori de invatare (Teaching Vectors).
* **Segmentare:** Grila de 9x28 este impartita in 4 zone de 9x7 pixeli, rezultand 63 de caracteristici (features) per neuron.
* **Fisiere:** `DateCifre.bin`, `DateLitere.bin`, `DateSimboluri.bin`.


## 4. Arhitectura SIA si State Machine (Etapa 4)

Aplicatia utilizeaza un design pattern de tip **State Machine** pentru a asigura un flux de date coerent si robust:
1.  **IDLE:** Starea de asteptare pentru input-ul utilizatorului pe Front Panel.
2.  **PROCESS:** Segmentarea matricii si transformarea datelor din 2D (9x7) in vectori 1D.
3.  **INFERENCE:** Utilizarea functiei `Calculate Response.vi` pentru a genera predictiile pe baza modelului.
4.  **DISPLAY:** Afisarea rezultatelor si a gradului de incredere in UI.


## 5. Antrenare, Optimizare si Experimente (Etapa 5 & 6)

S-au realizat **4 experimente de optimizare** pentru a identifica arhitectura ideala a retelei neuronale:

| Exp#  | Configuratie (Neuroni Hidden / LR) | Accuracy | F1-Score | Observatii                                            |
| :----:| :---------------------------------:| :-------:| :-------:| :----------------------------------------------------:|
| 1     | 10 Neuroni / LR 0.1                | 68%      | 0.62     | Rezultate sub pragul minim cerut.                     |
| 2     | 20 Neuroni / LR 0.1                | 74%      | 0.69     | Crestere vizibila a preciziei.                        |
| **3** | **30 Neuroni / LR 0.1**            | **82%**  | **0.78** | **Configuratie OPTIMA - Depaseste pragurile impuse.** |
| 4     | 30 Neuroni / LR 0.05               | 80%      | 0.77     | Stabilitate mare, dar necesita mai multe iteratii.    |

**Justificare:** S-a ales varianta cu **30 de neuroni** deoarece a oferit cel mai bun echilibru intre acuratete si capacitatea de a distinge literele complexe la rezolutie mica.

## 6. Rezultate Finale si Analiza Performantei (Results)

### 6.1 Performanta Sistemului
Sistemul a demonstrat o viteza de raspuns instantanee (< 50ms) si o stabilitate ridicata. Procesul de invatare a fost monitorizat prin graficul de eroare, care a convergent sub pragul de 0.1.

### 6.2 Acuratete si Limitari
* **Accuracy Finala:** 82% (Cerinta: ≥ 70%)
* **F1-Score Final:** 0.78 (Cerinta: ≥ 0.65)
* **Limitari:** Confuzia intre cifra '8' si litera 'B' din cauza densitatii similare de pixeli pe grila 9x7.

### 6.3 Optimizarea Parametrilor
Cea mai buna configuratie a fost stabilita la 30 de neuroni in stratul ascuns. S-a implementat un prag de incredere de 0.7 pentru a filtra rezultatele cu probabilitate scazuta.

### 6.4 Lectii Invatate
Am inteles ca succesul unui model RN in LabVIEW depinde critic de consistenta datelor de antrenare si de maparea corecta a intrarilor (Preprocesare).


## 7. Instructiuni de Rulare (Demonstratie End-to-End)

1.  Deschideti proiectul in **LabVIEW**.
2.  Verificati instalarea toolkit-ului **NI Super Simple Neural Networks**.
3.  Rulati aplicatia principala: `src/app/VI_Final/Antrenare neuroni ghicire cifre_litere_simboluri_grupuri de simboluri.vi`.
4.  Desenati pe grila booleana si verificati predictia.
5.  O demonstratie video este disponibila in folderul `docs/demo/`.


## 8. Structura Finala Repository (Tag: v0.6-optimized-final)

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