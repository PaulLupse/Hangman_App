# Hangman Application


<p align="justify">
Această aplicație prezintă o metodă de automatizare a arhicunoscutului joc pe hârtie 
numit "Spânzurătoarea" (în limba engleză, sub numele de "Hangman"). Pentru realizarea acesteia
a fost folosit limbajul Python în ultima sa versiune, deși aplicația rulează fără nici o problemă
folosind versiuni anterioare, de la versiunea 3.4 a limbajului.
</p>

## Descriere

<p align="justify">
Jocul presupune participarea a doi jucători, deși unele variante pot include mai mulți jucători. 
Pentru simplitate,
în cele ce urmează, considerăm doi participanti la joc: jucătorul și jucătorul advers.
O partidă constă în ghicirea unui cuvânt ascuns de către jucător, cuvântul fiind propus
de către jucătorul advers. Ghicirea cuvântului ascuns este asistată de abilitatea 
jucătorului de a sugera litere despre care speculează că ar putea fii în cuvântul 
ascuns. Dacă litera sugerată se află în cuvânt, toate aparițiile acesteia sunt 
dezvăluite. În caz contrar, jucătorul advers completează o reprezentare grafică a unui
om sub o spânzurătoare. Fiecare sugerare reprezintă o "încercare", iar dacă numărul
de încercări greșite (în care litera propusă nu se află în cuvânt) depășește un anumit
număr (de regulă, numărul total de părți ale corpului, necesare desenării omului de
sub spânzurătoare), jucătorul pierde partida.
</p>

## Conținut

1. Structură
2. Utilizare
3. Algoritm
4. Complexitate
5. Limitări

### 1. Structură 

<p align="justify">
Folder-ul de bază a aplicției conține script-ul principal, 'main.py', împreună cu licența, fișierul de 
cerințe, și alte 4 folder-uri: 'data', 'docs', 'results', 'src'.

Folder-ul 'src' conține script-ul pe care se bazează acest proiect, și anume 'WordGuesser.py', care
reține implementarea algoritmului de ghicire. 

Folder-ul 'data' conține resursele relevante rulării aplicației, precum dicționarul standard folosit de algoritmul
de ghicire. Tot în folder-ul 'data' este inclusă o sub-aplicație care implementează o interfață grafică minimă,
menită pentru facilitarea testării algoritmului, localizată în folder-ul 'Tester'. Sub-aplicația are capacitatea de a 
genera fișiere test, folosind dicționarul standard, care sunt memorate în folderul 'TestFiles' inclus în folder-ul 
data.

Rezultatele rulării algoritmului sunt stocate în folder-ul 'results', iar materialele de documentare adiționale sunt
localizate în folder-ul 'docs'.
</p>

### 2. Utilizare

#### Fișierele de intrare

<p align="justify">
Aplicația operează cu fișiere de intrare de tip CSV (comma-separated values). Fiecare linie din fișier reprezintă
o 'partidă' de joc, iar toate liniile din fișier trebuiesc să aibă exact 3 câmpuri:

- Primul câmp reprezină eticheta partidei de joc
- Al doilea câmp reprezintă cuvântul așa cum este văzut de către ghicitor, la care ne vom referi sub numele
  de 'cuvânt cenzurat', având următorul format:
  - Dacă un caracter din cuvânt este caracterul '*' (asterix), acesta reprezintă un caracter ascuns.
  - Dacă un caracter din cuvânt face parte din alfabetul românesc, acesta este un caracter vizibil ghicitorului
    de la începutul partidei. Dacă există mai multe apariții al acestui caracter în interiorul cuvântului, toate
    aceste apariții trebuie să fie vizibile de la început.
  - Dacă există vreun caracter '-' în cuvânt, acesta trebuie să fie vizibil de la început.
  - Oricare alt caracter este invalid, iar prezența acestuia poate duce la o eroare.
- Al treilea câmp reprezintă cuvântul ascuns ce trebuie ghicit
- Primul și al doilea câmp trebuie să fie de lungimi egale

</p>

#### Fișierele de ieșire

<p align="justify">
Algoritmul de ghicire memorează rezultatele rulării asupra unui fișier de intrare într-un fișier de ieșire de tip
CSV. Fiecare linie din fișierul de ieșire corespunde unei partide de joc, în afară de ultimele două linii, care 
servesc la afișarea tutror pașilor efectuați pe parcursul a tuturor partidelor, respectiv a mediei de pași efectuați.
O linie din fișierul de ieșire prezintă următoarele câmpuri, în ordinea în care apar:

- Eticheta partidei,
- Numărul de pași efectuați în acea partidă,
- Cuvântul ghicit,
- Lista de caractere încercate de către algoritm, în ordinea în care au fost încercate.
</p>


#### Rulare

<p align="justify">
Există mai multe metode de rulare a aplicației. Cea mai rapidă metodă este deschiderea fișierului 'main.py' localizat
în folder-ul de bază al aplicației. Metoda recomandată de rulare este prin intermediul unui terminal (de exemplu
Microsoft Powershell) folosind următoarea linie de comandă:
</p>

    python main.py 

<p align="justify">
Utilizatorul are, de asemenea, posibilitatea de a specifica fișierul de intrare de tip CSV conținând cuvintele
ce doresc a fii ghicite, fișierul de ieșire unde vor fii memorate rezultatele algoritmului, și fișierul dicționar 
conținând lista de cuvinte ce va fii folosită de algoritm în procesul de ghicire. O astfel de rulare arată în felul
următor:
</p>

    python main.py --input fișier_intrare --output fișier_ieșire --dict fișier_dicționar

<p align="justify">
Nu este necesară, totuși, specificarea tuturor opțiunilor. În lipsa acestora, aplicația va folosi fișierele
input, output și dicționar standard: data/TestFiles/DefaultTest.csv, results/DefaultResult.csv, 
respectiv data/RomanianWords.txt. Astfel, este posibilă rularea prin linii de comandă în format-ul următor:
</p>

    python main.py --input fișier_intrare
    
    python main.py --output fișier_ieșire

    python main.py --input fișier_intrare --dict fișier_dicționar

<p align="justify">
Rularea fără nici o opțiune specificată este echivalentă cu rularea folosind următoarea linie de comandă:
</p>

    python main.py --input data/TestFiles/DefaultTest.csv --output results/DefaultResult.csv --dict data/RomanianWords

<p align="justify">
Pentru rularea aplicației în modul testare, este necesară specificarea opțiunii prin intermediul argumentului
'mode', în felul următor:
</p>

    python main.py --mode test

<p align="justify">
Rularea aplicației în modul test ignoră automat celelalte argumente de rulare, întrucât sub-aplicația de testare
folosește doar fișierele intrare regăsite în data/TestFiles și dicționarul predefinit.
</p>

<p align="justify">
Atenție: Ștergerea acestor fișierelor de intrare/ieșire și dicționar predefinite sau relocarea
acestora din folder-urile în interiorul cărora se află duce nefuncționarea aplicației în modul test.
</p>

#### Modul 'test'

<p align="justify">
Modul 'test' oferă o interfață grafică minimă utilizatorului, prin intermediul căreia se poate alege, printre altele,
generarea de fișiere test, folosind dicționarul predefinit data/RomanianWords.txt. Meniurile din interfața grafică
sunt parcurse prin introducerea, de la tastatură, a unui număr corespunzător unei opțiuni prezentate pe ecran.

Rularea cu succes a aplicației în modul test va determina apariția a următorului text în terminal:
</p>

    Selectați o opțiune:
    1. Generare fișier test.
    2. Rulare algoritm pentru un fișier test.
    3. Ștergere fișier test.
    0. Ieșire.

<p align="justify">
Alegerea primei acțiuni determnă generarea unui fișier de test, memorat în folder-ul 'data/TestFiles'.

Alegerea celei de-a doua opțiuni determină apariția unui meniu de selectare a unui fișier, asupra căruia va fii
aplicat algoritmul.

Alegerea celei de-a treia opțiuni determină apariția unui meniu de selectare a unui fișier, care va fii șters în
urma selectării.

Cea de-a patra opțiune determină ieșirea din program.
</p>

### 3. Algoritm

<p align="justify">
Algoritmul de ghicire se bazează pe lista de cuvinte oferită prin intermediul fișierului dicționar, fie cel 
predefinit, fie cel precizat de către utilizator prin intermediul argumentului 'dict'.

Pentru fiecare partidă de joc în parte, înainte de a începe propunerea literelor de către algoritm, este efectuată
o căutare secvențială prin toată lista de cuvinte memorată. Toate cuvintele ce se potrivesc cuvâtului cenzurat sunt
adăugate într-o listă de candidați. Un cuvânt se potrivește cuvântului cenzurat dacă are același număr de litere și 
dacă oricare literă vizibilă din cuvântul cenzurat se regăsește și în cuvântul candidat, pe exact aceleeași poziție.

Algoritmul parcurge toți candidații, în ordinea în care au fost adăugați în listă, și sugerează, pe rând, literele
din interiorul acestora. Este reținută o listă cu literele anterior încercate, pentru evitarea risipirii încercărilor.
La finalul încercării literelor dintr-un candidat, acesta este eliminat din listă. Totdată, pe parcurs ce sunt 
găsite literele din cuvântul cenzurat, lista de candidați este micșorată prin reverificarea potrivirii a tutror
candidaților și eliminarea celor ce nu se mai potrivesc cu noul cuvânt cenzurat.

Scopul acestui algoritm este de a găsi, în cele din urmă, cuvântul ce trebuie ghicit, în lista de cuvinte. Dacă, 
în schimb, acest cuvânt nu este găsit, algoritmul folosește forța brută pentru a continua ghicirea cuvântului,
sugerând literele neîncercate până la acel moment, în ordinea frecvenței acestora în limba română.
</p>

### 4. Complexitate

<p align="justify">
Rezolvarea fiecărei partide de joc în parte, mai precis ghicirea fiecărui cuvânt în parte, nu depinde în nici o 
măsură de rezolvarea celorlalte partide de joc. Astfel, putem spune că este vorba despre o complexitate liniară
pentru terminarea tutror partidelor din interiorul unui fișier de intrare.

Dacă ne raportăm la o singură partidă, unde datele de intrare sunt literele ce trebuiesc ghicite, complexitatea 
variază drastic în funcție atât de ordinea în care sunt așezate în dicționar cuvintele, cât și de prezența cuvântului ce 
trebuie ghicit și locația acestuia în dicționar. 

Într-un caz ideal, în urma construirii listei de cuvinte candidate, primul cuvânt din acea listă este chiar cuvântul ce 
trebuie ghicit. În acest caz, sunt este efectuat un număr de pași egal cu numărul de litere ce trebuiesc ghicite. 

În cel mai rău caz, nici unul dintre cuvintele candidat nu conțin vreo literă din cuvântul ce trebuie ghicit. Astfel, se 
rezumă la aplicarea forței brute. Totodată, și aplicarea forței brute are o complexitate variabilă, în funcție de
literele din interiorul cuvântului ce trebuie ghicit. În cel mai rău caz, cuvântul conține cele mai puțin folosite
litere din limba română, ceea ce ar determina adăugarea a unui număr de pași egal cu restul de litere nefolosite
înainte de începerea aplicării forței brute. 

Cazurile în care cuvântul ce trebuie ghicit se află în interiorul dicționarului, prezintă, când este folosit
dicționarul predefinit, un număr mediu de 11 pași.
</p>

### 5. Limitări

<p align="justify">
Aplicația este limitată la limba română, deși adăugarea unei alte limbi este o sarcină trivială. 

Performanța algoritmului de ghicire este limitată la domeniul cuvintelor reprezentat de dicționarul care este folosit.
Performanța scade cu până la 50% în cazul în care nici un cuvânt din fișierul de intrare nu se află în dicționarul
folosit.
</p>

## Licență

MIT License