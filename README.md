# Proiect Retele Neuronale - Recunoastere Caractere in LabVIEW

**Student:** Perpelea George-Lucian  
**Disciplina:** Retele Neuronale (RN)  
**Institutie:** Universitatea POLITEHNICA Bucuresti - FIIR  
**Tehnologie:** LabVIEW + NI Super Simple Neural Networks

---

## 1. Descrierea Proiectului
Acest proiect implementeaza un Sistem cu Inteligenta Artificiala (SIA) capabil sa recunoasca si sa prezica un set de 4 caractere (cifre, litere sau simboluri) desenate de utilizator. 

Sistemul utilizeaza o interfata grafica bazata pe un array de booleene de **9 linii si 28 de coloane**. Grila este impartita automat in 4 sub-matrice (fiecare de 9x7 pixeli), care sunt procesate de o retea neuronala pentru a identifica simbolurile desenate manual.

---

## 2. Modulele Sistemului (SIA)
Proiectul este structurat in trei module principale, dezvoltate integral in LabVIEW:

1.  **Modulul de Generare Date:** Contine VI-urile care transforma desenele booleene in vectori de invatare (Teaching Vectors) si ii stocheaza in fisiere de tip `.bin`.
2.  **Modulul de Antrenare:** Utilizeaza libraria "NI Super Simple Neural Networks" pentru a antrena reteaua pe baza fisierelor binare generate.
3.  **Modulul de Predictie:** Aplicatia finala care preia desenul curent, il proceseaza prin reteaua antrenata si afiseaza rezultatul ghicit.


## 3. Structura Repository-ului
Perpelea-George-Lucian-RN/
├── data/                         # Fisiere de date si modelul antrenat
│   ├── DateCifre.bin             
│   ├── DateLitere.bin            
│   ├── DateSimboluri.bin         
│   └── trained_model.bin         # Modelul rezultat dupa antrenare
│
├── src/                          # Codul sursa LabVIEW (.vi)
│   ├── GenerareDate/             
│   │   ├── GenerareDateCifre.vi
│   │   ├── GenerareDateLitere.vi
│   │   └── GenerareDateSimboluri.vi
│   ├── Antrenare/                
│   │   ├── Antrenare neuroni ghicire numere.vi
│   │   ├── Antrenare neuroni ghicire litere.vi
│   │   ├── Antrenare neuroni ghicire simboluri.vi
│   │   └── SubVIs/               # Clase si API-uri Neural Network
│   └── VI_Final/                 
│       └── Antrenare neuroni ghicire cifre_litere_simboluri_grupuri de simboluri.vi
│
├── docs/                         # Documentatie si diagrame
│   ├── state_machine.png         # Diagrama de stari a proiectului
│   └── screenshots/              # Capturi de ecran cu interfata
│
├── etapa4_arhitectura_sia.md     
├── etapa5_antrenare_model.md     
├── etapa6_optimizare_concluzii.md 
└── README.md                     # Overview general (Acest fisier)

## 4. Analiza si Concluzii
  ## 4.1 Performanta Sistemului
  Sistemul SIA ofera o predictie stabila si un timp de raspuns instantaneu. In urma procesului de dezvoltare, s-a observat ca reteaua neuronala reuseste sa clasifice corect caracterele atata timp cat acestea respecta structura de baza invatata in faza de antrenare.

  ## 4.2 Acuratete si Limitari
  Acuratetea estimata prin testare manuala este de aproximativ 80-82%. Limitarile principale sunt legate de rezolutia grilei (9x7 pixeli per caracter), care poate duce la confuzii intre caractere cu forme asemanatoare (de exemplu, intre cifra '8' si litera 'B' sau cifra '0' si litera 'O').

  ## 4.3 Optimizarea Retelei
  Cea mai buna configuratie a fost stabilita la un numar de 30 de neuroni in stratul ascuns (Hidden Layer). Aceasta setare permite retelei sa aiba suficienta "memorie" pentru a distinge literele complexe, fara a incetini procesul de predictie sau a produce fenomene de supra-antrenare.

  ## 4.4 Lectii Invatate
  Dezvoltarea proiectului a demonstrat ca succesul predictiei depinde in mare masura de calitatea si diversitatea datelor de antrenare. Modul in care utilizatorul deseneaza caracterele in faza de generare (GenerareDate.vi) este factorul determinant pentru performanta finala a sistemului.

## 5. Instructiuni de Rulare
Deschideti proiectul in LabVIEW si asigurati-va ca aveti instalat toolkit-ul NI Super Simple Neural Networks.
Pentru Predictie: Rulati VI-ul din folderul src/VI_Final/. Desenati pe matricea de 9x28 si observati rezultatul.
Pentru Antrenare: Daca doriti sa reantrenati reteaua, folositi VI-urile din src/Antrenare/, indicand calea catre fisierele .bin corespunzatoare din folderul data/.
Pentru Generare Date: Puteti adauga noi exemple de desene folosind VI-urile din src/GenerareDate/ pentru a creste baza de date.
