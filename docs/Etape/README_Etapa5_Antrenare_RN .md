# README – Etapa 5: Configurarea si Antrenarea Modelului RN

**Disciplina:** Retele Neuronale  
**Institutie:** POLITEHNICA Bucuresti – FIIR  
**Student:** Perpelea George-Lucian  
**Link Repository GitHub:** https://github.com/lucispecialul324/Perpelea-George-Lucian-RN
**Data predarii:** 18/12/2025

## 1. Scopul Etape 5

Obiectivul acestei etape este antrenarea si validarea unei arhitecturi de retele neuronale modulare. Proiectul utilizeaza **trei retele neuronale specializate independente**, antrenate separat pentru a recunoaste categorii specifice de caractere:

1.  **Retea Cifre** 
2.  **Retea Litere** 
3.  **Retea Simboluri** 

Scopul final este integrarea acestor trei module de predictie intr-un **VI Principal** capabil sa interpreteze o secventa de pixeli (grupuri de simboluri) dintr-o matrice de intrare atipica de **9 linii x 28 coloane**.

## 2. Arhitectura Sistemului

Sistemul este construit pe principiul separarii problemelor, utilizand sub-VI-uri dedicate pentru antrenare (Training) si predictie (Inference).

### A. Componente de Antrenare (Back-end)
Fiecare categorie dispune de propriul VI de antrenare si propriul set de date binar, asigurand o specializare maxima a ponderilor neuronale:

* **Modul Cifre:**
    * VI Antrenare: `Antrenare neuroni ghicire numere.vi` 
    * Dataset: `DateCifre.bin`
* **Modul Litere:**
    * VI Antrenare: `Antrenare neuroni ghicire litere.vi` 
    * Dataset: `DateLitere.bin` 
* **Modul Simboluri:**
    * VI Antrenare: `Antrenare neuroni ghicire simboluri.vi` 
    * Dataset: `DateSimboluri.bin` 

### B. VI-ul Final de Integrare (Front-end)
Acesta reprezinta interfata principala a aplicatiei (Antrenare neuroni ghicire cifre_litere_simboluri_grupuri de simboluri.vi).

* **Input:** O matrice de pixeli (array 2D) de dimensiune 9x28.

* **Segmentare:** Folosind functia Array Subset, matricea mare este impartita automat in 4 sub-matrici egale de 9x7 pixeli. Aceasta permite introducerea a pana la 4 caractere simultan.

* **Selectie (Control):** Un control de tip Enum ("Ce tip desenam?") permite utilizatorului sa specifice contextul (Cifre, Litere sau Simboluri).

* **Predictie:** In functie de selectia din Enum, programul incarca fisierul .bin corespunzator si ruleaza predictia pentru fiecare dintre cele 4 segmente.

## 3. Pregatirea si Structura Datelor

Datele nu sunt unificate intr-un singur fisier, fiecare retea lucrand cu propriul spatiu de caracteristici.

* **Format Date:** Fisiere binare (`.bin`) ce contin vectori 1D (Flattened) proveniti din imagini rasterizate.
* **Generare:** Datele au fost create folosind generatoarele specifice: `GenerareDateCifre.vi`, `GenerareDateLitere.vi`, `GenerareDateSimboluri.vi`.
* **Dimensiune Input Retea:** Numarul de neuroni de intrare corespunde rezolutiei caracterelor individuale utilizate la antrenare.


## 4. Configurarea si Antrenarea (Per Modul)

Fiecare dintre cele 3 VI-uri de antrenare a fost configurat individual folosind toolkit-ul "Super Simple Neural Networks".

### Tabel Hiperparametri

| Hiperparametru    | Valoare (Cifre/Litere/Simboluri)   | Justificare                                                                                    |
| :----------------:| :---------------------------------:| :--------------------------------------------------------------------------------------------: |
| **Arhitectura**   | MLP (Multi-Layer Perceptron)       | Eficienta pentru pattern-uri statice cu complexitate medie.                                    |
| **Input Layer**   | Variabil                           | Corespunde vectorului de pixeli ai unui singur caracter.                                       |
| **Hidden Layer**  | 1 Strat (Create 1 Hidden Layer.vi) | Numarul de neuroni ascunsi a fost ajustat: mai putini pentru Cifre, mai multi pentru Simboluri.|
| **Output Layer**  | Corespunzator claselor             | Neuroni egali cu numarul de caractere din fiecare fisier `.bin`.                               |
| **Learning Rate** | 0.1 - 0.3                          | Setat experimental pentru a evita oscilatiile erorii vizibile in `Error Evaluation.ctl`.       |

**Rezultate Antrenare:**
Retelele au fost antrenate pana cand eroarea globala a scazut sub pragul acceptabil, iar ponderile au fost salvate in fisierele binare pentru a fi incarcate de VI-ul final.

## 5. Integrare si Testare Finala (Grupuri de Simboluri)

Testarea finala se realizeaza in VI-ul principal cu matricea **9x28**.

**Flux de executie:**

1. Utilizatorul selecteaza din Enum tipul de date pe care urmeaza sa le introduca (ex: "Cifre").
2. Utilizatorul deseneaza in controlul grafic de 9 linii x 28 coloane.
3. Algoritmul imparte imaginea in 4 ferestre de 9x7.
4. Fiecare fereastra este procesata de reteaua neuronala incarcata specific (cea de Cifre, conform selectiei).
5. Programul identifica indexul cu cea mai mare probabilitate pentru fiecare fereastra.
6. Rezultatul este afisat sub forma unui Array de valori/string-uri corespunzatoare celor 4 pozitii.


## 6. Analiza Erorilor

Arhitectura modulara prezinta avantaje si limitari:

1.  **Avantaj:** Flexibilitate. Daca recunoasterea simbolurilor este slaba, se poate re-antrena doar `Retea Simboluri` fara a afecta Cifrele sau Literele.
2.  **Limitare (Confuzii):** Caracterele similare grafic (0 vs O, 1 vs I) sunt dificil de diferentiat fara context, deoarece retelele decid strict matematic.
3.  **Limitare (Rezolutie):** Matricea 9x28 ofera o inaltime redusa (9 pixeli), ceea ce poate duce la pierderea detaliilor pentru simbolurile complexe.
4.  **Dependenta de Context:** Deoarece sistemul foloseste un selector manual (Enum), utilizatorul trebuie sa specifice corect ce deseneaza. Daca se selecteaza "Cifre" si se deseneaza litere, rezultatele vor fi eronate (reteaua va incerca sa aproximeze litera cu cea mai apropiata cifra).
5.  **Suprapunere:** Sistemul asteapta ca fiecare caracter sa fie centrat in fereastra sa de 9x7. Desenarea intre ferestre (pe liniile de demarcatie imaginare) poate duce la erori de segmentare.

## 7. Structura Repository - Etapa 5

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
├── README_Etapa4_Arhitectura_SIA.md  
└── README_Etapa5_Antrenare_RN.md