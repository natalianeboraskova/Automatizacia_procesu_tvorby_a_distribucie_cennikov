# Automatizacia procesu tvorby a distribucie cennikov
Toto riešenie bolo vytvorené s cieľom zautomatizovať celý proces prípravy, schvaľovania a distribúcie týždenných cenníkov zákazníkom. V minulosti bolo potrebné jednotlivé kroky vykonávať manuálne, čo zvyšovalo časovú náročnosť procesu a vytváralo priestor pre chyby spôsobené ľudským faktorom. 

## *Hlavné výhody riešenia*

•	Úspora času – odpadá potreba manuálneho generovania, kontroly a rozosielania cenníkov.

•	Zníženie rizika chýb – automatizované kroky minimalizujú nesprávne kopírovanie údajov alebo zabudnuté úlohy.

•	Kontrolovaný schvaľovací proces – každá zmena prechádza schválením zodpovednou osobou pred distribúciou zákazníkom.

•	Automatické upozornenia – v prípade chyby alebo zastavenia procesu sú okamžite odoslané notifikácie zodpovedným osobám.

•	Jednoduchá správa príjemcov – zoznam zákazníkov je možné aktualizovať priamo v Exceli bez zásahu do samotnej automatizácie.

•	Ochrana súkromia zákazníkov – každý zákazník dostáva cenník samostatne a nevidí ostatných príjemcov.

•	História cenníkov – automatické vytváranie kópií umožňuje jednoduché dohľadanie historických cien.

•	Vyššia spoľahlivosť procesu – pravidelné spúšťanie v presne definovanom čase zabezpečuje konzistentné doručovanie cien na ďalšie obdobie. 

Automatizácia tak prináša nielen významnú administratívnu úsporu, ale aj vyššiu kvalitu procesu, kontrolu nad schvaľovaním údajov a spoľahlivejšiu komunikáciu smerom k zákazníkom. Vďaka tomu sa môžu zodpovedné osoby venovať aktivitám s vyššou pridanou hodnotou namiesto opakujúcich sa manuálnych úloh

## *Pohľad zamestnanca*

Pre zamestnanca je celý proces veľmi jednoduchý a vyžaduje len minimálny zásah. Každý piatok ráno dostane schvaľovateľ email s odkazom na Excel súbor a inštrukciami na kontrolu aktuálnych kurzov a vypočítaných cien. Po overení údajov stačí zmeny uložiť a následne požiadavku schváliť alebo zamietnuť prostredníctvom schvaľovacieho emailu. 


Po schválení sa už o všetko ostatné postará automatizácia:
•	prenesie schválené údaje do cenníka,

•	pripraví finálny cenník na nasledujúci týždeň,

•	rozpošle ho všetkým zákazníkom,

•	vytvorí archívnu kópiu pre potreby histórie a auditu. 

Zamestnanec sa tak nemusí zaoberať manuálnym kopírovaním údajov, rozosielaním emailov ani archiváciou dokumentov. Jeho úlohou je iba skontrolovať správnosť cien a potvrdiť alebo zamietnuť ich publikovanie. 

-------------------------------------------------------------------------------------------------------------------------------------------------------------

## Technická stránka veci

<img width="424" height="896" alt="image" src="https://github.com/user-attachments/assets/76d62707-00c8-42fc-aca9-325e85f65151" />


Každý piatok o 5:00 sa spustí flow – v tomto čase očakávam 100% plynulosť chodu flowu. Zároveň bude to prvý email, ktorý dostane approver do emailu a v prípade chyby a zastavenia flowu, budem skorej informovaná automatickým emailom, ktorý sa nachádza na spodku flowu.
Vytvorí sa variable vo formáte string. Obsahuje funkciu ktorá vytvorí dátum tak, že keď sa spustí flow v piatok, tak pripočíta 2 dni k dnešnému dátumu = vytvorí dátum nedele. 
Vytvorime web-link na excel kde je tabulka s aktualizujúcim sa kurzom a prepočtom ceny. 
Následne sa pošle approval na approvera (osobu kt. má schváliť/odmietnuť požiadavku), s kompletným postupom ako postupovať, dynamickým obsahom (meniaci sa dátum atď.)   
Delay, aby sa stihli online excel zmeny zosynchronizovať s tými na sharepointe a naslede v power automate. 



<img width="475" height="290" alt="image" src="https://github.com/user-attachments/assets/9d9f82b0-bb98-41a1-96f3-dcbc5a59d3c3" />


Ak je approval schválený pred 9:00 – tak flow bude čakat do 9:00, ak bude approval schvalený neskorej ako 9hod. flow automaticky pokračuje ďalej.



<img width="433" height="969" alt="image" src="https://github.com/user-attachments/assets/a9b6c5b1-d0a1-4cad-9cc3-f6120e87fc36" />


Ak výsledok approvalu je „Reject“ pošle sa email na vybranú osobu. Ak je výsledok approvaluj „Accept“, načíta sa mi tabulka, ktorú upravil/skontroloval approver. Nakopírujú sa výsledné hodnoty do Cenníka, do príslušnej tabuľky. 
Aby sa stihli načítať správne hodnoty, manuálne zbehne delay – 2 minúty. 
Načítam si zo sharepointu excel Cenník.
Načíta emailové adresy z excelu Zákazníci, výhodou je, že emailové adresy vieme v exceli upraviť, vymazať, pridať. Špecialne spravené, tak, aby zákaznící dostávali každý email zvlášť a „nevideli“ sa navzájom.
Pošle sa email zákazníkovi s cenníkom s cenami na ďalší týždeň.



<img width="295" height="202" alt="image" src="https://github.com/user-attachments/assets/1a5077fb-8936-476b-be90-0dcbcc81d4a2" />


Zbehne excel script a spraví kopiu aktuálneho Cenníka. Zákazník aj firma si vedia pomocou týchto kopií pozrieť historické ceny.
Ako posledný článok flowu je email, ktorý sa odošle, ak vo flowe nastane chyba, zasekne sa a pod. a finalny email s cenníkom sa všetkým zákazníkom nepošle. Funguje ako automatická kontrola :)

## Problémy počas tvorby, negatíva
1.) Ako prvý problém vnímam, manuálne otvorenie excel súboru s kurzom, nakoľko excel nedokáže dáta obnovovať ak nie je daný súbor otvorený. To sa ale prirodzene poriešilo kontrolou zamestanca, ktorú treba vykonať, hlavne čo sa týka zmien a kontrola marží. Zo skúseností, marže sa menia veľmi často, tak súbor by bol otváraný bez ohladu na "problém" s kurzom. Pomerne detailom, je podmienka zakliknutia "enable content" aby sa kurz obnovil. Toto som poriešila postupom v approvali aby sa zabezpečil plynulý chod.

2.) Vo všeobecnosti, automatizácia a Power-automate sú veľmi citlivé na detaly a chyba napr. vo veľkom/malom pismene vedia flow zastaviť. V mojom prípade je flow cielene spravený tak, aby sa možnostiam pochybenia človeka úplne vyhlo a prenechali sme to na softvér. Problém môže nasťať len keď hlási chybu Microsoft. :)
















