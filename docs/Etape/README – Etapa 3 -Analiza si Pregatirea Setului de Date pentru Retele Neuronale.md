# README – Etapa 3: Analiza si Pregatirea Setului de Date

**Disciplina:** Retele Neuronale  
**Institutie:** POLITEHNICA Bucuresti – FIIR  
**Student:** Perpelea George-Lucian  
**Link Git-hub** https://github.com/lucispecialul324/Perpelea-George-Lucian-RN

## Introducere

Acest document descrie activitatile realizate in **Etapa 3**, concentrata pe generarea si preprocesarea datelor necesare pentru recunoasterea celor 4 caractere (cifre, litere, simboluri) desenate manual pe o grila de 9x28 pixeli. 


## 1. Structura Repository-ului (Versiunea Etapa 3)

Perpelea-George-Lucian-RN/
├── data/                         # Date brute si procesate
│   ├── DateCifre.bin             # Dataset cifre generat programatic
│   ├── DateLitere.bin            # Dataset litere generat programatic
│   └── DateSimboluri.bin         # Dataset simboluri generat programatic
├── src/
│   ├── GenerareDate/             # Modulele de achizitie date
│   │   ├── GenerareDateCifre.vi
│   │   ├── GenerareDateLitere.vi
│   │   └── GenerareDateSimboluri.vi
│   └── Antrenare/                # Pregatirea pentru etapa de training
└── docs/                         # Documentatie si imagini grila

## 2. Descrierea Setului de Date
 ## 2.1 Sursa datelor
 Origine: Generare programatica prin interfata LabVIEW (Front Panel).
 Modul de achizitie: Desen manual pe un array de booleene de 9 linii si 28 de coloane. 
 Proces: Caracterele sunt salvate sub forma de "Teaching Vectors" folosind API-ul Neural Network.

 ## 2.2 Caracteristicile dataset-ului
 Numar total de observatii: Variabil (minim 10-20 mostre per caracter).
 Numar de caracteristici (Features): 252 pixeli (9x28). Fiecare caracter individual ocupa un sub-array de 9x7 pixeli (63 caracteristici per neuron).
 Tipuri de date: Booleene (True/False) convertite in valori binare (1/0).
 Format fisiere: .bin (format binar optimizat pentru LabVIEW).

 ## 2.3 Descrierea caracteristicilor
 Caracteristica    Tip    Unitate    Descriere                       Domeniu valori
 Pixel (i, j)      Binar  bit        Starea butonului in grila       {0, 1}
 Selectie Tip      Cat.   -          Eticheta caracterului desenat   {0-9, A-Z, Simbol}

## 3. Analiza Exploratorie a Datelor (EDA)
 ## 3.1 Statistici descriptive aplicate
 Densitatea Pixelilor: Calcularea mediei de pixeli "1" per caracter pentru a distinge complexitatea formelor (ex: 'I' are densitate mica, '8' are densitate mare).
 Distributia valorilor: Histogramarea bitilor de 0 si 1 pentru a verifica echilibrul intre spatiul gol si cel desenat in matricea 9x28.
 
 ## 3.2 Analiza calitatii datelor
 Detectarea valorilor lipsa: 0%. LabVIEW preia starea butoanelor booleene, deci fiecare pixel are o valoare definita (0 sau 1).
 Consistenta: Verificarea concordantei dintre desenul manual si eticheta aleasa din vectorul de selectie inainte de salvarea in .bin.

 ## 3.3 Probleme identificate
 Variabilitate redusa: Riscul de a antrena reteaua doar pe desene "perfecte".
 Confuzii structurale: Similitudinea mare intre cifra '0' si litera 'O' la rezolutia mica de 9x7 pixeli per caracter.

## 4. Preprocesarea Datelor
 ## 4.1 Curatarea datelor
 Eliminarea zgomotului: Filtrarea pixelilor activati accidental in afara zonei de desen.
 Validarea intrarilor: VI-ul de generare asigura ca nu se salveaza matrici complet goale.

 ## 4.2 Transformarea caracteristicilor
 Flattening: Transformarea matricii 2D (9x28) intr-un vector 1D necesar pentru intrarea in neuronii retelei.
 Segmentare: Impartirea vectorului mare in 4 sub-vectori corespunzatori celor 4 caractere desenate.

 ## 4.3 Structurarea seturilor de date
 Train Set: Datele stocate in DateLitere.bin, DateCifre.bin, etc.
 Test Set: Testare in timp real prin desenare directa in VI-ul final.

 ## 4.4 Salvarea rezultatelor preprocesarii
 Parametrii si vectorii de invatare sunt salvati folosind functia Write Teaching Vectors.vi in folderul data/.

## 5. Fisiere Generate in Aceasta Etapa
data/DateCifre.bin – setul de date pentru numere.
data/DateLitere.bin – setul de date pentru litere.
src/GenerareDate/GenerareDateLitere.vi – codul de achizitie date.

## 6. Stare Etapa (Checklist)
[x] Structura repository configurata.
[x] Matricea 9x28 analizata pentru features.
[x] Modulele de generare date (.vi) functionale.
[x] Fisiere binare de date create in folderul data/.
