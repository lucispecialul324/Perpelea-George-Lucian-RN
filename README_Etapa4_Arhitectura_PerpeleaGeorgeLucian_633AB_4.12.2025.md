 README – Etapa 4: Arhitectura Completa a Aplicatiei SIA bazata pe Retele Neuronale
Disciplina: Retele Neuronale   Institutie: POLITEHNICA Bucuresti – FIIR   Student: Perpelea George-Lucian   Grupa: 633AB Link Repository GitHub: [ADAUGAȚI AICI LINK-UL CATRE REPOSITORY] Data: 04.12.2025  
Scopul Etapei 4
Aceasta etapa livreaza un SCHELET COMPLET si FUNCTIONAL al intregului Sistem cu Inteligenta Artificiala (SIA) implementat in LabVIEW, demonstrand integrarea modulelor de achizitie, preprocesare si model RN.
 Livrabile Obligatorii
1. Tabelul Nevoie Reala → Solutie SIA → Modul Software (max ½ pagina)
Proiectul se incadreaza in Controlul Calitatii in Productie.
Nevoie reala concreta	Cum o rezolva SIA-ul vostru	Modul software responsabil
Validarea rapida a marcajelor de lot/simbolurilor pe panouri HMI.	Clasificare pattern binar (string) → Decizie OK/Eroare in < 100 ms	RN Module (Antrenare neuroni...vi) + UI
Asigurarea invariantei la usoare variatii de pozitie sau deformare a simbolurilor.	Clasificare bazata pe MLP, robust la zgomot, cu acuratete > 99% (post-antrenare).	RN Module (Teach.vi si Calculate Response.vi)
Testarea flexibilitatii sistemului la recunoasterea grupurilor de simboluri (secvente).	Concatenare matrici booleene pentru a clasifica intregul grup ca o singura clasa unica.	Data Logging/Acquisition (GenerareDateSimboluri.vi)

2. Contributia Voastra Originala la Setul de Date – MINIM 40% din Totalul Observatiilor Finale
Proiectul se bazeaza pe generarea completa a pattern-urilor (simboluri, grupuri de simboluri) de catre student, prin definirea acestora in format String si conversia lor in Vectori 1D de pixeli.
Total observatii finale: [N] (Suma tuturor claselor din Cifre.bin + Litere.bin + Simboluri.bin) Observatii originale: [N] (100%)
Tipul contributiei: [X] Date generate prin simulare fizica (Simularea pattern-urilor binare pe grila)   [ ] Date achizitionate cu senzori proprii   [ ] Etichetare/adnotare manuala   [ ] Date sintetice prin metode avansate  
Descriere detaliata: Datele sunt 100% originale si generate intern, structurate pe trei categorii distincte:
1.	Cifre (0-9): Generate de GenerareDateCifre.vi in Cifre.bin.
2.	Litere (ex: A-Z): Generate de GenerareDateLitere.vi in Litere.bin.
3.	Simboluri (ex: '+', etc.): Generate de GenerareDateSimboluri.vi in Simboluri.bin.
Aceasta modularizare asigura ca antrenarea retelei se poate face incremental, testand performanta pe subseturi de date bine definite, inainte de antrenarea pe setul unificat.
Locatia codului: src/data_acquisition/ (contine cele 3 VI-uri de generare) 
Locatia datelor: data/processed/ (contine cele 3 fisiere .bin)
Dovezi:
•	Tabel Statistici: Variatia este concentrata pe distributia binară (0/1) a pixelilor si pe numarul de pixeli aprinsi pentru fiecare simbol/grup.
•	Setup experimental: Panourile Frontale LabVIEW demonstreaza intrarea String si iesirea Matrice Booleana pentru fiecare tip de data.

3. Diagrama State Machine a Intregului Sistem (OBLIGATORIE)
Proiectul se bazeaza pe ciclul de Clasificare la Cerere (Triggered Classification), adaptat la un singur ciclu de predictie declansat de utilizator.
Locatie Diagrama: docs/state_machine.png (PNG/SVG)
Starile principale:
1.	IDLE (Asteptare): Sistemul asteapta o intrare de la utilizator.
2.	ACQUIRE_INPUT: Utilizatorul introduce String-ul de prezis (ex: "A" sau "34").
3.	PREPROCESS: VI-ul corespunzator ruleaza conversia String → Matrice Booleana → Vector 1D.
4.	INFERENCE: Antrenare neuroni...vi (prin Calculate Response.vi) executa predictia pe Vectorul 1D folosind modelul unic sau modelul corespunzator categoriei.
5.	CLASSIFY_AND_DISPLAY: Sistemul extrage clasa si o afiseaza pe Panoul Frontal.
6.	LOG_AND_LOOP: Rezultatul predictiei este logat (optional) si sistemul revine in starea IDLE.
7.	ERROR: Stare de gestionare a erorilor.
Legenda obligatorie (scrieti in README):
Am ales arhitectura de Clasificare la Cerere (Triggered Classification) deoarece proiectul nostru (validarea simbolurilor) nu necesita monitorizare continua, ci o clasificare rapida la fiecare input nou.
Starile principale sunt:
1.	IDLE: Sistemul LabVIEW este pornit, dar asteapta introducerea unui String valid.
2.	PREPROCESS: Stare critica in care se asigura formatarea corecta a String-ului in Vector 1D (pe baza datelor din cele 3 .bin folosite la antrenare).
3.	INFERENCE: Executia modelului MLP.
4.	CLASSIFY_AND_DISPLAY: Extrage clasa unica (indexul elementului True) din Vectorul Boolean de iesire (One-Hot) si o afiseaza utilizatorului.
Tranzitia critica este din INFERENCE catre ERROR: Cand modelul RN returneaza un raspuns neconcludent (ex: probabilitati prea mici sau mai multe elemente True), indicand o eroare de clasificare sau de sistem.
Bucla de feedback este simpla: Dupa afisarea rezultatului (LOG_AND_LOOP), sistemul revine automat la IDLE pentru a permite o noua introducere de simbol.

4. Scheletul Complet al celor 3 Module Cerute la Curs
Toate modulele sunt implementate in LabVIEW si sunt functionale ca schelet.
Modul	LabVIEW VI	Cerinta minima functionala (la predare)
1. Data Logging / Acquisition	Cele 3 VI-uri de generare	MUST: Fiecare VI produce un fisier .bin si genereaza Vectorul 1D in timp real la predictie.
2. Neural Network Module	Antrenare neuroni ghicire numere.vi	MUST: Modelul MLP este definit si compilat (Create Network.vi).
3. Web Service / UI	Panoul Frontal al Antrenare neuroni...vi	MUST: Primeste input (String) si afiseaza output (Matrice Booleana 2D + Predictie One-Hot).
Detalii per modul (LabVIEW):
Modul 1: Data Logging / Acquisition (VI-uri separate)
Functionalitati obligatorii:
•	[X] Codul celor 3 VI-uri ruleaza fara erori.
•	[X] Producerea datelor este modularizata pe Cifre.bin, Litere.bin, Simboluri.bin.
•	[X] Logica de conversie String → Vector 1D este implementata corect in fiecare VI.
Modul 2: Neural Network Module (Antrenare neuroni ghicire numere.vi)
Functionalitati obligatorii:
•	[X] Arhitectura MLP definita si compilata.
•	[X] Include logica de incarcare a datelor (din cele 3 .bin sau un set unitar) pentru antrenare (Teach.vi).
•	[X] Include logica de predictie (Calculate Response.vi).
Modul 3: Web Service / UI (Panoul Frontal)
Functionalitati MINIME obligatorii:
•	[X] Control String pentru intrarea String-ului.
•	[X] Indicator Matrice Booleana 2D pentru a vizualiza pattern-ul desenat.
•	[X] Indicator Vector Boolean (One-Hot) sau Indicator Numeric pentru a afisa rezultatul predictiei.
•	[ ] Screenshot demonstrativ in docs/screenshots/ui_demo.png (de adaugat).
Structura Repository-ului la Finalul Etapei 4 (OBLIGATORIE)
(Se va asigura ca fisierele LabVIEW si documentele sunt plasate in structura de mai jos, unde VI-urile sunt in folderele src/)
proiect-rn-perpelea-lucian/
├── data/
│   ├── raw/
│   ├── processed/
│       ├── Cifre.bin     # Date transformate (Cifre)
│       ├── Litere.bin    # Date transformate (Litere)
│       └── Simboluri.bin # Date transformate (Simboluri/Grupuri)
│   ├── generated/
│   ├── train/
│   ├── validation/
│   └── test/
├── src/
│   ├── data_acquisition/
│       ├── GenerareDateCifre.vi     
│       ├── GenerareDateLitere.vi    
│       └── GenerareDateSimboluri.vi 
│   ├── preprocessing/  
│   ├── neural_network/
│       └── Antrenare neuroni ghicire numere.vi  
│   └── app/  # Panoul Frontal al VI-ului de antrenare serveste ca schelet UI 
├── docs/
│   ├── state_machine.png           # Diagrama State Machine
│   └── screenshots/
│       └── ui_demo.png
├── models/
├── config/
├── README.md
├── README_Etapa3.md
└── README_Etapa4_Arhitectura_SIA.md              # ← Acest fisier
Checklist Final – Bifati Totul Inainte de Predare
Documentatie si Structura
•	[X] Tabelul Nevoie → Solutie → Modul complet
•	[X] Declaratie contributie 40% date originale completata
•	[X] Cod generare/achizitie date functional si documentat (cele 3 VI-uri)
•	[ ] Dovezi contributie originala: grafice + log + statistici in docs/ (de adaugat)
•	[ ] Diagrama State Machine creata si salvata in docs/state_machine.* (de adaugat)
•	[X] Legenda State Machine scrisa (Sectiunea 3)
•	[X] Repository structurat conform modelului
Modul 1: Data Logging / Acquisition
•	[X] Codul celor 3 VI-uri ruleaza fara erori
•	[X] Producerea datelor este modularizata pe Cifre.bin, Litere.bin, Simboluri.bin
•	[X] Logica de conversie String → Vector 1D este implementata corect in fiecare VI.
Modul 2: Neural Network
•	[X] Arhitectura RN definita si documentata in cod (Antrenare neuroni...vi)
Modul 3: Web Service / UI
•	[X] Propunere Interfata ce porneste fara erori (Panoul Frontal)
•	[ ] Screenshot demonstrativ in docs/screenshots/ui_demo.png (de adaugat)

